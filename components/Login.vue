<template>
  <div class="main">
    <img src="../static/bank_logo.png" alt="Logo" />
    <h1>Login</h1>
    <form @submit="submitHandler">
      <input v-model="email" type="text" placeholder="Enter email" autofocus />
      <input v-model="password" type="password" placeholder="Password" />
      <button>{{ isLoading ? "Loading..." : "Login" }}</button>
    </form>
    <span class="error-show">{{ errorText }}</span>
  </div>
</template>

<script>
export default {
  data() {
    return {
      email: "",
      password: "",
      errorText: "",
      isLoading: false,
    };
  },
  methods: {
    async submitHandler(e) {
      e.preventDefault();
      let formData = {
        email: this.email,
        password: this.password,
      };
      try {
        this.isLoading = true;
        let response = await this.$auth.loginWith("local", { data: formData });
        localStorage.setItem("mytoken", `Bearer ${response.data.token}`);
        this.$auth.$storage.setUniversal("user", response.data.iam.email, true);
        this.$auth.setUser(response.data.iam.email);
        this.isLoading = false;
        if (response.status === 200) {
          this.$router.push("/dashboard");
        }
      } catch (err) {
        this.isLoading = true;
        const errorMessage = err.response && err.response.data.message;
        // console.log(err.response.data);
        // alert(errorMessage);
        this.errorText = errorMessage;
        this.isLoading = false;
      }
    },
  },
  // middleware({ $auth, redirect }) {
  //   if ($auth.loggedIn) {
  //     this.$router.push("/dashboard");
  //   }
  // },
  mounted() {
    let token = localStorage.getItem("mytoken");
    console.log(token);
    if (token) {
      this.$router.push("/dashboard");
    }
    if (token == null) {
      this.$router.push("/");
    }
  },
};
</script>

<style scoped>
.main {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 40px;
}
img {
  width: 100px;
}
form input {
  width: 300px;
  height: 40px;
  padding-left: 20px;
  display: block;
  margin-bottom: 30px;
  margin-right: auto;
  margin-left: auto;
  border: 1px solid skyblue;
  outline: none;
}
button {
  width: 320px;
  height: 40px;
  border: 1px solid skyblue;
  background: skyblue;
  color: #fff;
  cursor: pointer;
}
.error-show {
  color: red;
  padding: 5px;
}
</style>
