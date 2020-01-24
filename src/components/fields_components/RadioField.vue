<template>
  <div id="radio-field">
    <div>
      <label id="radio-field-label" for="name">{{radio_field.label}}</label>
      <div
        id="radio-button"
        v-for="(rf_op,i) in getArrayValueFromObject(radio_field.options[0])"
        v-bind:key="i"
      >
        <input type="radio" v-bind:id="i" v-bind:value="rf_op" v-model="selected" />
        <label v-bind:for="i">{{rf_op}}</label>
      </div>
      <p v-if="isFieldEmpty">{{radio_field.validation_message}}</p>
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
      console.log("array value", Object.values(obj));
      return Object.values(obj);
    },
    // method for field validation check
    checkIsFieldEmpty: function() {
      if (this.selected == "") {
        this.isFieldEmpty = true;
      } else this.isFieldEmpty = false;
    }
  },
  created() {
    // listenting form submit button using event bus
    bus.$on("submitClicked", data => {
      console.log("get from radio selection field");
      this.checkIsFieldEmpty();
      let fieldName = this.radio_field.label;
      console.log("selected options", this.selected);
      this.$store.state.formData.push({ [fieldName]: this.selected });
      console.log("store form data", this.$store.state.formData);
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
}
</style>