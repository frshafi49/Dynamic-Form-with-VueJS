<template>
  <div id="name_field">
    <div>
      <label for="name">{{name_field.label}}</label>
      <input
        id="name"
        v-bind:name="name_field.name"
        v-bind:type="name_field.email"
        v-bind:required="name_field.required"
        v-bind:placeholder="name_field.placeholder"
      />
      <p>Name is required</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    // pop for receiving name field requirement
    name_field: {
      type: Object
    }
  }
};
</script>

<style scoped>
#name_field {
  display: flex;
  align-items: flex-start;
}
#name_field label {
  padding: 10px;
  display: inline-block;
  width: 100px;
}
#name_field input {
  width: 200px;
  height: 20px;
}
#name_field p {
  margin: 10px;
  display: inline;
  color: red;
}
</style>