<template>

  <div>
    <div class="container-1">
      <div class="card">
        <div v-if="currentPost">
          <img :src="currentPost.image" alt="Product Image">
        </div>
        <div class="content" v-if="currentPost">
          <h1 :class="getChangeColorTitle">{{ currentPost.title }}</h1>
          <div class="category">
            <h4>{{ currentPost.category }}</h4>
            <h4>Rating</h4>
          </div>
          <hr class="horizontal-line">
          <h3>{{ currentPost.description }}</h3>
          <hr class="horizontal-line">
          <h2>${{ currentPost.price }}</h2>
          <div class="containt">
            <button :class="getChangeColorBuy" class="button-p">Buy Now</button>
            <button :class="getChangeColorNext" class="next-btn" @click="nextPost">Next Product</button>
          </div>
        </div>
      </div>
    </div>
    <div class="container-2">

    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue';

const posts = ref([]);
const currentPostIndex = ref(0);
const perPage = 1;

const fetchData = async () => {
  try {
    const response = await fetch('https://fakestoreapi.com/products');
    const data = await response.json();
    posts.value = data;
    console.log(data)
  } catch (error) {
    console.error('Error fetching data:', error);
  }
};

onMounted(() => {
  fetchData();
});

const getChangeColorTitle = computed(() => {
  return currentPost.value.category == "women's clothing" ? 'pink-title' : 'normal-title';
})
const getChangeColorBuy = computed(() => {
  return currentPost.value.category == "women's clothing" ? 'pink-buy-btn' : 'button-p';
})
const getChangeColorNext = computed(() => {
  return currentPost.value.category == "women's clothing" ? 'pink-buy-btn-next' : 'next-btn';
})

const currentPost = computed(() => {
  return posts.value[currentPostIndex.value];
});

const nextPost = () => {
  currentPostIndex.value = (currentPostIndex.value + 1) % posts.value.length;
};
</script>

<style scoped>
.normal-title {
  font-weight: 600;
  font-size: 25px;
  position: relative;
  margin-bottom: 40px;
  color: var(--primary-color-men);
}

.pink-title{
  font-weight: 600;
  font-size: 25px;
  position: relative;
  margin-bottom: 40px;
  color: var(--primary-color-women);
}

img{
  width: 300px;
  height: 400px;
  
}

.category{
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  margin-top: 5px;
  margin-bottom: 10px;
}

.horizontal-line {
   width: 100%;
}

h2{
  font-size: 26px;
  margin-top: 5px;
}

h3 {
  font-size: 18px;
  font-weight: 400px;
  opacity: 80%;
  margin-bottom: 35px;
  display: -webkit-box;
  -webkit-line-clamp: 5;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
h4{
  font-size: 18px;
  opacity: 70%;
}
.content{
  display: flex; 
  flex-direction: column; 
  justify-content: space-around; 
  width: 530px;
  height: 400px;
  align-items: flex-start;
}

.containt{
  display: flex;
  justify-content: space-around;
  width: 100%;
}

/* button */
.button-p{
  width: 250px;
  height: 40px;
  margin-block: 20px;
  border-radius: 5px;
  background-color: var(--primary-color-men);
  border: none;
  color: white;
  font-size: 17px;
}
.next-btn{
  width: 250px;
  height: 40px;
  margin-block: 20px;
  border-radius: 5px;
  border: 2px solid var(--primary-color-men);
  color: var(--primary-color-men);
  font-size: 17px;
  cursor: pointer;
}

.pink-buy-btn{
  width: 250px;
  height: 40px;
  margin-block: 20px;
  border-radius: 5px;
  background-color: var(--primary-color-women);
  border: none;
  color: white;
  font-size: 17px;
}

.pink-buy-btn-next{
  width: 250px;
  height: 40px;
  margin-block: 20px;
  border-radius: 5px;
  border: 2px solid var(--primary-color-women);
  color: var(--primary-color-women);
  font-size: 17px;
  cursor: pointer;
}
/* button */

.card {
    display: flex;
    justify-content: space-around;
    align-items: center;
    border-radius: 20px;
    background-color: white;
    width: 1053px;
    height: 580px;
}

.container-1{
  background-color: #FDE2FF;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 500px;
}

.container-2{
  background-color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 765px;
}

</style>