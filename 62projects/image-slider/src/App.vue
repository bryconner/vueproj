<template>
  <div>
       <img :src="image" />
   </div>
</template>

<script>
const images = [
    "https://i.picsum.photos/id/1/200/300.jpg?hmac=jH5bDkLr6Tgy3oAg5khKCHeunZMHq0ehBZr6vGifPLY",
    "https://i.picsum.photos/id/2/200/300.jpg?hmac=HiDjvfge5yCzj935PIMj1qOf4KtvrfqWX3j4z1huDaU",
    "https://i.picsum.photos/id/3/200/300.jpg?hmac=o1-38H2y96Nm7qbRf8Aua54lF97OFQSHR41ATNErqFc",
];
export default {
    name:"App",
    data() {
        return { index: 0, image: images[0]};
    },
methods: {
    next() {
        this.index = (this.index + 1) % images.length;
        this.image = images[this.index];
    },
    autoChangeSlide() {
        setInterval(() => {
            this.next();
        }, 1000);
    },
  },
  beforeMount() {
     this.autoChangeSlide(); 
  },   
};
</script>