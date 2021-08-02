<template>
  <h2>Draggable Elements</h2>
  <div
    class="draggable"
    :draggable="true"
    @dragstart="drag($event, o)"
    v-for="o of origin"
    :key="o"
    @click.stop
    :transfer-data="{}"
  >
    {{ o }}
  </div>

<h2>Target</h2>
  <div id="target" @dragover.prevent @drop="drop">
    <div class="draggable" v-for="t of target" :key="t">
      {{ t }}
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      origin: ["apple", "orange", "grape",],
      target: [],
    };
  },
  methods: {
    drag(ev, text) {
      ev.dataTransfer.setData("text", text);
    },
    drop(ev) {
      const text = ev.dataTransfer.getData("text");
      const index = this.origin.findIndex((o) => o === text);
      this.origin.splice(index, 1);
      this.target.push(text);
    },
  },
};
</script>

<style scoped>
.draggable {
  border: 1px solid black;
  margin-right: 5px;
}

#target {
  border: 1px solid black;
  width: 95vw;
  height: 100px;
  padding: 5px;
}
</style>

Au-Yeung, John. 62 Vue 3 Beginner Projects (pp. 264-266). Kindle Edition. 