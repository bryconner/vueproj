<template>
  <div>
    <button @click.stop="displayModal = true">open modal</button>
    <div id="modal" v-if="displayModal">
      <button @click="displayModal = false">x</button>
      <div>
          <button @click="prev">&lt;</button>
          <img :src="image" />
          <button @click="next">&gt;</button>
      </div>
    </div>
  </div>
</template>

<script>
  const images = [
  "https://images.dog.ceo/breeds/pomeranian/n02112018_5091.jpg",
  "https://images.dog.ceo/breeds/mountain-swiss/n02107574_753.jpg",
  "https://images.dog.ceo/breeds/leonberg/n02111129_3028.jpg",
  ];
  export default {
    name: "App",
    data() {
        return { index: 0, image: images[0], displayModal: false };
    },
    methods: {
      next() {
        this.index = (this.index + 1) % images.length;
        this.image = images[this.index];
      },
      prev() {
        this.index = (this.index - 1) % images.length;
        this.image = images[this.index];
      },
      onClickOutsode(e) {
        if (e.target.localName !== "button") {
          this.displayModal = false;
        }
      },
    },
    mounted() {
      window.addEventListener("click", this.onClickOutsode);
    },
  };
</script>

<style scoped>
  img {
      width: 200px;
      height: 200px;
  }
  #modal {
    position: absolute;
    top: 20px;
    left: 20px;
    border: 1px solid gray;
    background-color: white;
  }  
</style>