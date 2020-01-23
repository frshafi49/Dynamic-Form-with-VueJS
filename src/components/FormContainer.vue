<template>
  <div id="form-container">
    <div v-for="(field,i) in fields" v-bind:key="i">
      <div v-if="field.type=='text'">
        <Name v-bind:name_field="field" />
      </div>
      <div v-else-if="field.type=='email'">
        <Email v-bind:email_field="field" />
      </div>

    </div>
  </div>
</template>

<script>

// including form fields components
import Name from "../components/fields_components/Name.vue";
import Email from "../components/fields_components/Email.vue";

export default {
  name: "app",
  components: {
    Name,
    Email
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


      ]
    };
  }
};
</script>

<style>
</style>
