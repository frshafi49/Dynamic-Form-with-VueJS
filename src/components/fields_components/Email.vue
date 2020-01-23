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
      />
      <p>{{email_field.validation_message}}</p>
    </div>
  </div>
</template>

<script>
export default {

  props: {
    // pop for receiving email field requirement
    email_field: {
      type: Object
    }
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
  width: 100px;
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