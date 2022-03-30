<template>
  <div v-if="status">
    <div class="model" @click.self="toggleModel">
      <div class="model__container">
        <div class="model__form">
          <form @submit="submitHandler">
            <div class="model__group">
              <h3>Fill the {{ handle }} form</h3>
            </div>
            <div class="model__group">
              <input
                type="date"
                class="model__control"
                placeholder="Date"
                v-model="date"
                required
              />
            </div>
            <div class="model__group">
              <input
                type="text"
                class="model__control"
                placeholder="Remarks"
                v-model="remarks"
              />
            </div>
            <div class="model__group">
              <input
                type="number"
                class="model__control"
                placeholder="Amount"
                v-model="amount"
                required
              />
            </div>
            <div class="model__group">
              <input type="submit" value="Add" class="button" />
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Modal",
  props: {
    status: Boolean,
    handle: String,
  },
  data() {
    return {
      date: "",
      remarks: "",
      amount: null,
      type: "",
      total: null,
      bankData: [],
    };
  },
  methods: {
    toggleModel() {
      this.$emit("model-toggle");
    },
    submitHandler(e) {
      e.preventDefault();
      if (this.handle == "debit") {
        this.bankData.push({
          date: this.date,
          remarks: this.remarks,
          amount: parseFloat(this.amount),
          type: "debit",
        });
        let totalBalance = {
          total:
            parseFloat((this.total -= this.amount)) || this.total - this.amount,
        };
        this.$emit("form-added");
        this.$emit("model-toggle");
        (this.date = ""), (this.remarks = ""), (this.amount = null);
        localStorage.setItem("trans-data", JSON.stringify(this.bankData));
        localStorage.setItem("total-balance", JSON.stringify(totalBalance));
      } else {
        this.bankData.push({
          date: this.date,
          remarks: this.remarks,
          amount: parseFloat(this.amount),
          type: "credit",
        });

        let totalBalance = {
          total:
            parseFloat((this.total += this.amount)) ||
            this.total + parseFloat(this.amount),
        };
        console.log(totalBalance.total, typeof totalBalance);
        this.$emit("form-added");
        this.$emit("model-toggle");
        (this.date = ""), (this.remarks = ""), (this.amount = null);
        localStorage.setItem("trans-data", JSON.stringify(this.bankData));
        localStorage.setItem("total-balance", JSON.stringify(totalBalance));
      }
    },
  },
  mounted() {
    if (process.browser) {
      this.total = JSON.parse(localStorage.getItem("total-balance")) || "0";
      this.bankData = JSON.parse(localStorage.getItem("trans-data") || "[]");
    }
  },
};
</script>

<style scoped>
.model {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 99;
}
.model__container {
  background: #fff;
  width: 450px;
  padding: 20px;
  border-radius: 5px;
}
.model__group {
  margin: 15px 0;
}
.model__control {
  border-bottom: 1px solid silver;
  border-left: none;
  border-right: none;
  border-top: none;
  width: 100%;
  padding: 7px 0;
  outline: none;
}
.button {
  border: none;
  border-radius: 3px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  background: skyblue;
  color: #fff;
  padding: 10px 15px;
  cursor: pointer;
  width: 100%;
  outline: none;
}
.button:hover {
  background: yellow;
  color: blue;
}
</style>
