<template>
  <div>
    <Header />
    <div class="breadcrumb"><h2>Dashboard</h2></div>
    <div class="btn-btn">
      <Button
        @model-show="showModelHandlerCredit"
        text="Add Credit"
        color="green"
        type="credit"
      />
      <Button
        @model-show="showModelHandlerDebit"
        text="Add Debit"
        color="red"
        type="debit"
      />
    </div>
    <Modal
      @model-toggle="showModelHandlerDebit"
      @model-toggle-credit="showModelHandlerCredit"
      :status="showModel"
      :handle="type"
      @form-added="updateTableData"
    />
    <TableData :key="componentKey" />
  </div>
</template>

<script>
import Header from "@/components/Header";
import Button from "@/components/Button";
import Modal from "@/components/Modal";
import TableData from "@/components/TableData";
export default {
  components: {
    Header,
    Button,
    Modal,
    TableData,
  },
  data() {
    return {
      loggedInUser: "",
      showModel: false,
      componentKey: 0,
      type: "",
    };
  },
  methods: {
    showModelHandlerDebit() {
      this.type = "debit";
      this.showModel = !this.showModel;
    },
    showModelHandlerCredit() {
      this.type = "credit";
      this.showModel = !this.showModel;
    },
    updateTableData() {
      this.componentKey += 1;
    },
  },
  middleware({ $auth, redirect }) {
    if (!$auth.loggedIn) {
      return redirect("/");
    }
  },
  mounted() {
    let token = localStorage.getItem("mytoken");
    if (token == null) {
      this.$router.push("/");
    }
  },
};
</script>
<style scoped>
.btn-btn {
  display: flex;
  justify-content: center;
}
.breadcrumb {
  padding: 5px;
  color: rgb(88, 88, 228);
}
</style>
