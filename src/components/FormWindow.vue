<template>
  <div id="form-window">
    <FormContainer />
    <DataTable v-if="validationPassed"></DataTable>
  </div>
</template>

<script>

// components
import FormContainer from "./FormContainer";
import DataTable from "./DataTable";

// event bus
import { bus } from "../main";

export default {
  components: {
    FormContainer,
    DataTable
  },

  data() {
    return {
      validationPassed: true
    };
  },
  created() {

    // listening form valid status
    // data table visibility depends on it
    bus.$on("PassedValidation", data => {
      console.log("required field validation passed", data);
      this.validationPassed = data;

    });
  }
};
</script>

<style scoped>
</style>
