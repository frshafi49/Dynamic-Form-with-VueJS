<template>
  <div id="selection-field">
    <div class="p-multiselect-label-container">
      <label for="name">{{multi_selection_field.label}}</label>
      <MultiSelect
        id="multiselect"
        v-model="selectedOptions"
        :options="getValueFromObejct(multi_selection_field.options[0])"
        optionLabel="brand"
        :placeholder="multi_selection_field.placeholder"
      />
      <br />
      <p
        v-if="isFieldEmpty && multi_selection_field.required"
      >{{multi_selection_field.validation_message}}</p>
    </div>
  </div>
</template>

<script>
import MultiSelect from "primevue/multiselect";
import { bus } from "../../main";
import { store } from "../../store/store";

export default {
  components: {
    MultiSelect
  },
  props: {
    // prop for receiving Selection field requirement
    multi_selection_field: {
      type: Object
    }
  },
  data() {
    return {
      selectedOptions: null,
      isFieldEmpty: false,
      isFieldRequired: this.multi_selection_field.required
    };
  },
  methods: {
    // get modified option list to cope up with primevue multi select
    getValueFromObejct: function(obj) {
      let value = Object.values(obj);
      let customSelectVal = [];
      value.forEach(el => {
        customSelectVal.push({ brand: el });
      });
      return customSelectVal;
    },

    // method for field validation check
    checkIsFieldEmpty: function() {
      if (this.selectedOptions == null) {
        this.isFieldEmpty = true;
      } else this.isFieldEmpty = false;
    },

    // method for check if required field empty
    // used in event bus
    isRequiredFieldEmpty: function() {
      return (
        this.multi_selection_field.required &&
        (this.selectedOptions == null || this.selectedOptions.lenght == 0)
      );
    }
  },
  created() {
    // listenting form submit button using event bus
    bus.$on("submitClicked", data => {
      this.checkIsFieldEmpty();

      // passing required field event
      if (this.isRequiredFieldEmpty()) {
        bus.$emit("PassedValidation", false);
      }

      // put data in store if field is not empty
      if (!this.isFieldEmpty) {
        let fieldName = this.multi_selection_field.label;
        this.$store.state.formData.push({ [fieldName]: this.selectedOptions });
      }
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
  margin-left: 150px;
}

#multiselect {
  vertical-align: middle;
}
</style>
<style  src="primevue/resources/primevue.min.css"></style>
<style  src="primevue/resources/themes/nova-light/theme.css"></style>
<style  src="primeicons/primeicons.css"></style>