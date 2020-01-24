<template>
  <div id="email-field">
    <div>
      <label for="email">{{email_field.label}}</label>
      <input
        id="email"
        v-bind:name="email_field.name"
        v-bind:type="email_field.email"
        v-bind:required="email_field.required"
        v-bind:placeholder="email_field.placeholder"
        v-model="email"
      />
      <p v-if="isFieldEmpty && email_field.required">{{email_field.validation_message}}</p>
    </div>
  </div>
</template>

<script>
import { bus } from "../../main";
import { store } from "../../store/store";

export default {
  props: {
    // prop for receiving email field requirement
    email_field: {
      type: Object
    }
  },
  data() {
    return {
      email: "",
      isFieldEmpty: false,
      isFieldRequired: this.email_field.required
    };
  },
  methods: {
    // method for field validation check
    checkIsFieldEmpty: function() {
      if (this.email == "") {
        this.isFieldEmpty = true;
      } else this.isFieldEmpty = false;
    },

    // method for check if required field empty
    // used in event bus
    isRequiredFieldEmpty: function() {
      return this.email_field.required && this.email == "";
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
        let fieldName = this.email_field.label;
        this.$store.state.formData.push({ [fieldName]: this.email });
      }
    });
  }
};
</script>

<style scoped>
#email-field {
  display: flex;
  align-items: flex-start;
}
#email-field label {
  padding: 10px;
  display: inline-block;
  width: 150px;
}
#email-field input {
  width: 200px;
  height: 20px;
  padding: 15px 5px 15px 5px;
  font-size: 15px;
}
#email-field p {
  margin: 10px;
  display: inline;
  color: red;
}
</style>