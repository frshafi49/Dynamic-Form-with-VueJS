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
      <p v-if="isFieldEmpty">{{email_field.validation_message}}</p>
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
      isFieldEmpty: false
    };
  },
  methods: {
    // method for field validation check
    checkIsFieldEmpty: function() {
      if (this.email == "") {
        this.isFieldEmpty = true;
      } else this.isFieldEmpty = false;
    }
  },
  created() {
    // listenting form submit button using event bus
    bus.$on("submitClicked", data => {
      console.log("get from email field");
      this.checkIsFieldEmpty();
      let fieldName = this.email_field.label;
      this.$store.state.formData.push({ [fieldName]: this.email });
      console.log("store form data", this.$store.state.formData);

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
}
#email-field p {
  margin: 10px;
  display: inline;
  color: red;
}
</style>