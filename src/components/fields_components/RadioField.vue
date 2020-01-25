<template>
  <div id="radio-field">
    <div>
      <label id="radio-field-label" for="name">{{radio_field.label}}</label>
      <div
        id="radio-button"
        v-for="(rf_op,i) in getArrayValueFromObject(radio_field.options[0])"
        v-bind:key="i"
      >
        <input type="radio" v-bind:id="rf_op" v-bind:value="rf_op" v-model="selected" />
        <label v-bind:for="rf_op">{{rf_op}}</label>
      </div>
      <br>
      <p v-if="isFieldEmpty && radio_field.required ">{{radio_field.validation_message}}</p>
    </div>
  </div>
</template>

<script>
import { bus } from "../../main";
import { store } from "../../store/store";

export default {
  props: {
    // prop for receiving name field requirement
    radio_field: {
      type: Object
    }
  },
  data() {
    return {
      selected: "",
      isFieldEmpty: false
    };
  },
  methods: {
    // method for convertion radio options into array
    getArrayValueFromObject: function(obj) {
      return Object.values(obj);
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
      return this.radio_field.required && this.selected == "";
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
        let fieldName = this.radio_field.label;
        this.$store.state.formData.push({ [fieldName]: this.selected });
      }
    });
  }
};
</script>

<style scoped>
#radio-field {
  display: flex;
  align-items: flex-start;
}
#radio-field-label {
  padding: 10px;
  display: inline-block;
  width: 150px;
}

#radio-button {
  display: inline-block;
}

#radio-field p {
  margin: 10px;
  display: inline;
  color: red;
  margin-left: 150px;
}
</style>