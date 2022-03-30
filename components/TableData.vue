<template>
  <div v-if="isDataAvailable" class="table_container">
    <h2>Transactions</h2>
    <div v-if="total.total < 0" class="fordebit">
      <h3>Total Balance:&nbsp;{{ numberWithCommas(total.total) }}</h3>
    </div>
    <div v-else class="forcredit">
      <h3>Total Balance:&nbsp;{{ numberWithCommas(total.total) }}</h3>
    </div>
    <table>
      <tr>
        <th>Date</th>
        <th>Amount</th>
        <th>Type</th>
        <th>Remarks</th>
      </tr>
      <tr
        v-for="result in bankData"
        :key="result.remarks"
        :class="
          result.type == 'credit' ? 'creditbackground' : 'debitbackground'
        "
      >
        <td>
          <NuxtLink :to="`/${result.date}`">
            {{ result.date }}
          </NuxtLink>
        </td>
        <td>{{ result.amount }}</td>
        <td>{{ result.type }}</td>
        <td>{{ result.remarks }}</td>
      </tr>
    </table>
  </div>
  <div v-else class="conditional_container">
    <span>seems, like you have no transactions yet</span>
  </div>
</template>
<script>
export default {
  name: "TableData",
  props: {
    showData: String,
    name: String,
  },
  data() {
    return {
      bankData: [],
      total: null,
      isDataAvailable: false,
    };
  },
  methods: {
    getTransactions() {
      if (localStorage.getItem("trans-data")) {
        this.bankData = JSON.parse(localStorage.getItem("trans-data"));
        this.isDataAvailable = true;
      }
      if (localStorage.getItem("total-balance")) {
        this.total = JSON.parse(localStorage.getItem("total-balance"));
      }
    },
    numberWithCommas(x) {
      return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    },
  },
  // created() {
  //   this.bankData = JSON.parse(localStorage.getItem("trans-data") || "[]");
  //   // console.log(this.bankData);
  // },
  mounted() {
    this.getTransactions();
  },
};
</script>

<style scoped>
.table_container {
  margin: 20px;
}
.conditional_container {
  /* text-align: center; */
  height: 50vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}
td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

h2 {
  line-height: 40px;
}
h3 {
  margin: 5px;
}
.fordebit {
  color: red;
}
.forcredit {
  color: green;
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
