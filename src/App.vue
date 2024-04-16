<template>
  <div>
    <ImageCarousel :images="imageList" />
  </div>
</template>

<script>
import ImageCarousel from './components/ImageCarousel.vue';

export default {
  components: {
    ImageCarousel
  },
  data() {
    return {
      imageList: [] 
    };
  },
  async created() {
    const imageUrl = 'https://picsum.photos/200/300';
    try {
      const response = await fetch('https://picsum.photos/v2/list');
      const data = await response.json();
  
      this.imageList = data.map(item => `${imageUrl}?id=${item.id}`);
      console.log(this.imageList)
    } catch (error) {
      console.error('Сталася помилка під час отримання зображень:', error);
    }
  }
};
</script>