<template>
  <div id="name-field">
    <div>
      <label for="name">{{name_field.label}}</label>
      <input
        id="name"
        v-bind:name="name_field.name"
        v-bind:type="name_field.email"
        v-bind:placeholder="name_field.placeholder"
        v-model="name"
      />
      <p v-if="isFieldEmpty && name_field.required">{{name_field.validation_message}}</p>
    </div>
  </div>
</template>

<script>
import { bus } from "../../main";
import { store } from "../../store/store";

export default {
  props: {
    // prop for receiving name field requirement
    name_field: {
      type: Object
    }
  },
  data() {
    return {
      name: "",
      isFieldEmpty: false,
      isFieldRequired: this.name_field.required
    };
  },
  methods: {
    // method for field validation check
    checkIsFieldEmpty: function() {
      if (this.name == "") {
        this.isFieldEmpty = true;
      } else this.isFieldEmpty = false;
    },

    // method for check if required field empty
    // used in event bus
    isRequiredFieldEmpty: function() {
      return this.name_field.required && this.name == "";
    }
  },
  created() {
    console.log("is text field required", this.isFieldRequired);
    console.log("is text field empty", this.isFieldEmpty);

    // listenting form submit button using event bus
    bus.$on("submitClicked", data => {
      // field validtaion check
      this.checkIsFieldEmpty();

      console.log("entered here text");

      if (this.isRequiredFieldEmpty()) {
        console.log("required re failed text");
        bus.$emit("PassedValidation", false);
      }

      // put data in store if field is not empty
      if (!this.isFieldEmpty) {
        let fieldName = this.name_field.label;
        this.$store.state.formData.push({ [fieldName]: this.name });
        console.log("store form data", this.$store.state.formData);
      }
    });
  }
};
</script>

<style scoped>
#name-field {
  display: flex;
  align-items: flex-start;
}
#name-field label {
  padding: 10px;
  display: inline-block;
  width: 150px;
}
#name-field input {
  width: 200px;
  height: 20px;
  padding: 15px 5px 15px 5px;
  font-size: 15px;
}
#name-field p {
  margin: 10px;
  display: inline;
  color: red;
}
</style>