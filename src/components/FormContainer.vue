<template>
  <!-- render specific field component based on field type  -->
  <div id="form-container">
    <div>
      <div v-for="(field,i) in fields" v-bind:key="i">
        <div v-if="field.type=='text'">
          <NameField v-bind:name_field="field" />
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
         <button v-on:click.prevent="onSubmitClicked()">Submit form</button>
      </div>
    </div>
  </div>
</template>

<script>
// including form fields components
import NameField from "../components/fields_components/NameField.vue";
import EmailField from "../components/fields_components/EmailField.vue";
import MultiSelectionField from "../components/fields_components/MultiSelectionField.vue";
import SingleSelectionField from "../components/fields_components/SingleSelectionField.vue";
import RadioField from "../components/fields_components/RadioField.vue";

// accessing event bus
import {bus} from '../main';

export default {

  name: "app",
  components: {
    NameField,
    EmailField,
    MultiSelectionField,
    SingleSelectionField,
    RadioField
  },
  data() {
    return {
      fields: [
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
          required: false,
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
          name: "status",
          type: "radio",
          label: "Status",
          required: true,
          validation_message: "Status is required",
          options: [
            {
              valid: "Valid",
              invalid: "Invalid"
            }
          ]
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
          name: "status",
          type: "radio",
          label: "Status",
          required: true,
          validation_message: "Status is required",
          options: [
            {
              valid: "Valid",
              invalid: "Invalid"
            }
          ]
        }
      ]
    };
  },
  methods:{
      onSubmitClicked:function(){
          bus.$emit('submitClicked');
      }
  }
};
</script>

<style scoped>
/* #form-container {

} */
</style>
