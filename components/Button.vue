<template>
  <div class="container">
    <div>
      <button :style="{ background: color }" class="btn" @click="modelHandler">
        <b>{{ text }}</b>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Button",
  data() {
    return {
      //   type: "debit",
    };
  },
  methods: {
    modelHandler() {
      if (this.type == "debit") {
        this.$emit("model-show");
      }
      if (this.type == "credit") {
        // alert("credit");
        this.$emit("model-show");
      }
    },
  },
  props: {
    text: String,
    color: String,
    type: String,
  },
  emits: ["model-show"],
};
</script>

<style scoped>
.btn {
  border: none;
  border-radius: 3px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  background: green;
  color: #fff;
  padding: 10px 15px;
  cursor: pointer;
  outline: none;
}
.container {
  text-align: center;
  margin: 20px;
}
</style>
