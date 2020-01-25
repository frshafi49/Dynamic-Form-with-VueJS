<template>
  <div id="selection-field">
    <div>
      <label for="name">{{single_selection_field.label}}</label>
      <Dropdown
        v-model="selected"
        :options="getValueFromObejct(single_selection_field.options[0])"
        optionLabel="name"
        :placeholder="single_selection_field.label"
      />
      <br/>
      <p
        v-if="isFieldEmpty && single_selection_field.required"
      >{{single_selection_field.validation_message}}</p>
    </div>
  </div>
</template>

<script>
import Dropdown from "primevue/dropdown";
import { bus } from "../../main";
import { store } from "../../store/store";

export default {
  components: {
    Dropdown
  },
  props: {
    // prop for receiving Selection field requirement
    single_selection_field: {
      type: Object
    }
  },
  data() {
    return {
      selected: "",
      isFieldEmpty: false,
      isFieldRequired: this.single_selection_field.required
    };
  },
  methods: {
    getValueFromObejct: function(obj) {
      let value = Object.values(obj);
      let customSelectVal = [];
      value.forEach(el => {
        customSelectVal.push({ name: el });
      });
      console.log("customSelectVal", customSelectVal);
      return customSelectVal;
    },
    // method for field validation check
    checkIsFieldEmpty: function() {
      if (this.selected == "") {
        this.isFieldEmpty = true;
      } else this.isFieldEmpty = false;
    },
    // method for check if required field empty
    // used in event bus
    isRequiredFieldEmpty: function() {
      return this.single_selection_field.required && this.selected == "";
    }
  },
  created() {
    // listenting form submit button using event bus
    bus.$on("submitClicked", data => {
      this.checkIsFieldEmpty();

      // passing require field event
      if (this.isRequiredFieldEmpty()) {
        bus.$emit("PassedValidation", false);
      }

      // put data in store if field is not empty
      if (!this.isFieldEmpty) {
        let fieldName = this.single_selection_field.label;

        // get value of the selected object
        let key = Object.keys(this.selected)[0];
        let value = this.selected[key];
        this.$store.state.formData.push({ [fieldName]: value });
      }
    });
  },
  updated() {}
};
</script>

<style scoped>
#selection-field {
  display: flex;
  align-items: flex-start;
}
#selection-field label {
  padding: 10px;
  display: inline-block;
  width: 150px;
}
#selection-field input {
  width: 200px;
  height: 20px;
}
#selection-field p {
  margin: 10px;
  display: inline;
  color: red;
  margin-left: 150px;
}

#multiselect {
  vertical-align: middle;
}
</style>
// <style  src="primevue/resources/primevue.min.css"></style>
// <style  src="primevue/resources/themes/nova-light/theme.css"></style>
// <style  src="primeicons/primeicons.css"></style>