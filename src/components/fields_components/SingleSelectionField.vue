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
    </div>
    <p v-if="isFieldEmpty">{{single_selection_field.validation_message}}</p>
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
      selected: null,
      isFieldEmpty: false
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
      if (this.selected == null) {
        this.isFieldEmpty = true;
      } else this.isFieldEmpty = false;
    }
  },
  created() {
    // listenting form submit button using event bus
    bus.$on("submitClicked", data => {
      console.log("get from single selection field");
      this.checkIsFieldEmpty();
      let fieldName = this.single_selection_field.label;
      console.log("selected", this.selected);
      this.$store.state.formData.push({ [fieldName]: this.selected });
      console.log("store form data", this.$store.state.formData);
    });
  }
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
}

#multiselect {
  vertical-align: middle;
}
</style>
// <style  src="primevue/resources/primevue.min.css"></style>
// <style  src="primevue/resources/themes/nova-light/theme.css"></style>
// <style  src="primeicons/primeicons.css"></style>