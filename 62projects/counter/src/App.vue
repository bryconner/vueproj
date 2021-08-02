<template>
  <div>
    <button v-on:click="count++">increment</button>
    <button v-on:click="count--">decrement</button>
    <p>{{ count }}</p>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return { count: 0 };
  },
};
</script>