<template>
  <div>
    <div class="info">
      <h4>All Transactions For</h4>
      {{ $route.params.id }}
      <h5>{{ date }}</h5>
    </div>
    <table v-if="bankData">
      <tr class="header">
        <th>Date</th>
        <th>Amount</th>
        <th>Type</th>
        <th>Remarks</th>
      </tr>
      <tr
        v-for="result in modifiedData"
        :key="result.remarks"
        :class="
          result.type == 'credit' ? 'creditbackground' : 'debitbackground'
        "
      >
        <td>
          {{ result.date }}
        </td>
        <td>{{ result.amount }}</td>
        <td>{{ result.type }}</td>
        <td>{{ result.remarks }}</td>
      </tr>
      <tr></tr>
    </table>
  </div>
</template>

<script>
export default {
  name: "TableDetails",
  components: {},
  data() {
    return {
      date: this.$route.params.id,
      bankData: [],
      modifiedData: [],
    };
  },
  methods: {
    getTransactions() {
      if (localStorage.getItem("trans-data")) {
        this.bankData = JSON.parse(localStorage.getItem("trans-data"));
        this.modifiedData = this.bankData.filter((el) => el.date == this.date);
      }
    },
  },
  beforeMount() {
    if (process.browser) {
      this.getTransactions();
    }
  },
};
</script>

<style scoped>
.info {
  margin: 10px;
}
.info > h5 {
  margin: 10px;
  color: green;
  font-weight: bold;
}
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 90%;
  margin-left: 30px;
}
td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}
.debitbackground {
  background-color: rgb(247, 230, 230);
  border-left: 4px solid red;
}
.creditbackground {
  background-color: rgb(204, 233, 204);
  border-left: 4px solid green;
}
</style>
