<template>
  <nav>
    <div>
      <h5>MY BANK</h5>
      <img src="../static/bank_logo.png" alt="Logo" />
    </div>
    <h4>
      Welcome, {{ user }}
      <!-- Welcome,&nbsp;{{ name }}&nbsp; -->
      <span @click="logoutHandler"><b>(Logout)</b></span>
    </h4>
  </nav>
</template>

<script>
export default {
  name: "Header",
  props: {
    name: String,
  },
  methods: {
    logoutHandler() {
      if (confirm("Are you sure ?")) {
        this.$auth.$storage.removeUniversal("user");
        localStorage.clear();

        this.$router.push("/");
      }
    },
  },
  computed: {
    user: function () {
      return this.$auth.$storage.getUniversal("user");
    },
  },
};
</script>

<style scoped>
nav {
  width: 100%;
  height: 60px;
  background: #fff;
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.1);
  display: flex;
  justify-content: space-around;
  align-items: center;
}
div {
  display: flex;
  justify-content: center;
  align-items: center;
}
div > img {
  width: 60px;
}
span:hover {
  color: green;
  cursor: pointer;
}
</style>
