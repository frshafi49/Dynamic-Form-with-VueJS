<template>
  <!-- show table based on form data -->
  <div id="data-table">
    <table>
      <tr>
        <th v-for="(tblHeader,i) in getTableHeader(fields)" v-bind:key="i">{{tblHeader}}</th>
      </tr>
      <tr>
        <td v-for="(tblBody,i) in getTableValue(fields)" v-bind:key="i">{{tblBody}}</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      fields: []
    };
  },
  methods: {
    // method for loading form submission data from store
    getFormDataFromStroe: function() {
      this.fields = this.$store.state.formData;
    },

    //method for extracting table header from data
    getTableHeader: function(fields) {
      let tblHeader = [];
      fields.forEach(element => {
        let key = Object.keys(element)[0];
        tblHeader.push(key);
      });
      return tblHeader;
    },

    // method for extracting table body
    getTableValue: function(fields) {
      let tblBody = [];
      // reading each element of form
      fields.forEach(element => {
        // reading key and value
        let key = Object.keys(element)[0];
        let value = element[key];

        // if any object is array, add them in array
        if (Array.isArray(value)) {
          let concVal = [];
          value.forEach(val => {
            let key = Object.keys(val)[0];
            let value = val[key];
            concVal.push(value);
          });
          //seperate them with comma
          let finalVal = concVal.join(",");
          tblBody.push(finalVal);
        }
        // if not array direcly add
        else {
          tblBody.push(value);
        }
      });
      return tblBody;
    }
  },
  created() {
    //loading store form submission data
    this.getFormDataFromStroe();
    // this.getTableValue(this.fields);
  }
};
</script>

<style scoped>
#data-table {
  /* background-color:#d7ccc8; */
  padding: 20px 20px 20px 20px;
}

table,
th,
td {
  border: 1px solid black;
  border-collapse: collapse;
}
th,
td {
  padding: 15px;
  text-align: left;
}
table#t01 tr:nth-child(even) {
  background-color: #eee;
}
table#t01 tr:nth-child(odd) {
  background-color: #fff;
}
table#t01 th {
  background-color: black;
  color: white;
}
</style>
