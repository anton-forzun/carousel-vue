<template>
  <div class="image-carousel">
    <h1>Carousel</h1>
    <div class="images-container" :style="{ width: containerWidth + 'px', transform: `translateX(-${currentIndex * imageWidth}px)` }">
      <img v-for="(image, index) in images" :src="image" :key="index" class="image" :class="{ 'selected': selectedImages.includes(image) }" @click="toggleSelected(image)" />
    </div>
    <div class="buttons-container">
      <button @click="scrollLeft" class="prev-button">Prev</button>
      <button @click="scrollRight" class="next-button">Next</button>
    </div>
    <div class="selected-images" v-if="selectedImages.length > 0">
      <p>Обрані зображення:</p>
      <ul>
        <li v-for="(image, index) in selectedImages" :key="index" class="selected-image">
          <img :src="image" alt="" class="img"/>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    images: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      currentIndex: 0,
      containerWidth: window.innerWidth,
      imageWidth: 200,
      selectedImages: []
    };
  },
  mounted() {
    window.addEventListener('resize', this.updateContainerWidth);
   
  },
  beforeUnmount() {
    window.removeEventListener('resize', this.updateContainerWidth);
  },
  methods: {
    scrollLeft() {
      this.currentIndex = (this.currentIndex - 1 + this.images.length) % this.images.length;
    },
    scrollRight() {
      this.currentIndex = (this.currentIndex + 1) % this.images.length;
    },
    updateContainerWidth() {
      this.containerWidth = window.innerWidth;
    },
    toggleSelected(imageUrl) {
      if (this.selectedImages.includes(imageUrl)) {
        this.selectedImages = this.selectedImages.filter(url => url !== imageUrl);
      } else {
        this.selectedImages.push(imageUrl);
      }
    }
  }
};
</script>

<style scoped>
h1 {
  text-align: center;
}
.image-carousel {
  position: relative;
  overflow: hidden;
}

.images-container {
  margin-top:50px ;
  position: relative;
  display: flex;
  gap: 10px;
  transition: transform 0.5s ease; 
}

.image {
  width: 100%;
  border-radius: 10px;
  height: auto;
  cursor: pointer;
  
}

.selected {
  border: 2px solid rgb(90, 90, 216); 
  opacity: 0.7;
  box-shadow: 1px -1px 16px 0px rgba(0,0,0,0.75);
-webkit-box-shadow: 1px -1px 16px 0px rgba(0,0,0,0.75);
-moz-box-shadow: 1px -1px 16px 0px rgba(0,0,0,0.75);
}

.buttons-container {
  margin-top: 20px;
  display: flex;
  justify-content: space-between;
  padding: 0 20px;
}

.prev-button,
.next-button {
  background-color: transparent;
  border: none;
  cursor: pointer;
}

.prev-button {
  border-radius: 30px;
  background: #d1c6c6;
  padding: 10px;
  
  left: 10px;
}

.next-button {
  right: 10px;
  border-radius: 30px;
  background: #d1c6c6;
  padding: 10px;
}

.prev-button:hover{
  opacity: 0.7;
}
.next-button:hover{
  opacity: 0.7;
}
.prev-button:active{
  position: relative;
  left: 1px;
}
.next-button:active{
  position: relative;
   left: 1px;
}

.selected-images {
  margin-top: 40px;
  padding: 0 30px;
}


.selected-images p {
  margin-bottom: 15px;
  text-align: center;
  font-size: 20px;
}

.selected-images ul {
  list-style: none;
  padding: 0;
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  justify-content: center;
}

.selected-image {
  margin-right: 10px;
  margin-bottom: 5px;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #f9f9f9;
  transition: opacity 0.3s ease; 
}
@media (max-width: 768px) {
  .selected-images {
    padding: 0 5px;
  }
  .images-container {
    justify-content: center
  }
}
</style>