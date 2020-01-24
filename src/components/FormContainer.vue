<template>
  <!-- render specific field component based on field type  -->
  <div id="form-container">
    <div>
      <div v-for="(field,i) in fields" v-bind:key="i">
        <div v-if="field.type=='info_html'">
          <FormIntro v-bind:info_field="field" />
        </div>
        <div v-if="field.type=='text'">
          <TextField v-bind:name_field="field" />
        </div>
        <div v-else-if="field.type=='email'">
          <EmailField v-bind:email_field="field" />
        </div>
        <div v-else-if="field.type=='multi-select'">
          <MultiSelectionField v-bind:multi_selection_field="field" />
        </div>
        <div v-else-if="field.type=='select'">
          <SingleSelectionField v-bind:single_selection_field="field" />
        </div>
        <div v-else-if="field.type=='radio'">
          <RadioField v-bind:radio_field="field" />
        </div>
      </div>
      <div>
        <button v-on:click.prevent="onSubmitClicked()">{{buttonTxt}}</button>
      </div>
    </div>
  </div>
</template>

<script>
// including form fields components
import FormIntro from "../components/fields_components/FormIntro";
import TextField from "../components/fields_components/TextField";
import EmailField from "../components/fields_components/EmailField";
import MultiSelectionField from "../components/fields_components/MultiSelectionField";
import SingleSelectionField from "../components/fields_components/SingleSelectionField";
import RadioField from "../components/fields_components/RadioField";

// accessing event bus
import { bus } from "../main";

export default {
  components: {
    FormIntro,
    TextField,
    EmailField,
    MultiSelectionField,
    SingleSelectionField,
    RadioField
  },
  data() {
    return {
      buttonTxt: "Submit Form",
      isDisable: false,
      fields: [
        {
          type: "info_html",
          content: "<h3>Please fill up the following form</h3>"
        },
        {
          name: "first_name",
          type: "text",
          label: "First Name",
          placeholder: "First Name",
          required: true,
          validation_message: "First name is required"
        },
        {
          name: "email",
          label: "Email",
          type: "email",
          placeholder: "Email",
          required: true,
          validation_message: "Email is required"
        },
        {
          name: "ocupation",
          type: "multi-select",
          label: "Ocupation",
          placeholder: "Select Ocupation",
          required: true,
          validation_message: "Ocupation is required",
          options: [
            {
              doctor: "Doctor",
              engineer: "Engineer",
              teacher: "Teacher",
              other: "Other"
            }
          ]
        },
        {
          name: "internal_status",
          type: "select",
          label: "Internal Status",
          required: true,
          validation_message: "Internal Status is required",
          options: [
            {
              valid: "Valid",
              invalid: "Invalid"
            }
          ]
        },
        {
          name: "jobtype",
          type: "radio",
          label: "Job Type",
          required: true,
          validation_message: "JobType is required",
          options: [
            {
              valid: "Full",
              invalid: "Part time"
            }
          ]
        }
      ]
    };
  },
  methods: {
    // event fires during submit button pressed
    onSubmitClicked: function() {
      bus.$emit("submitClicked");
    }
  },
  created() {
    // listening form valid status
    // submit button functionality depends on it
    bus.$on("PassedValidation", data => {
      console.log("required field validation passed", data);
      this.validationPassed = data;
      console.log('validation passed',this.validationPassed);

    });
  },
  updated(){

  }
};
</script>

<style scoped>

#form-container{
    background-color:#9e9e9e;
    padding: 20px 20px 20px 20px;
    max-width: 600px;
    border-radius: 10px;
    margin: 20px;
}

button{
    padding: 10px;
    margin-left:8px;
    margin-top: 10px;
    background-color: #555555;
    font-size: 16px;
    color: white;

}
</style>
