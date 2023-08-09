<template>
  <div :class="getChangeBackground" v-if="currentPost">
    <div v-if="shouldShowCard" :class="getChangeCard" class="card">
      <div v-if="currentPost">
        <img :src="currentPost.image" alt="Product Image">
      </div>
      <div class="content" v-if="currentPost">
        <h1 :class="getChangeColorTitle">{{ currentPost.title }}</h1>
        <div class="category">
          <h4>{{ currentPost.category }}</h4>
          <h4>{{ currentPost.rating.rate }}</h4>
        </div>
        <hr class="horizontal-line">
        <h3>{{ currentPost.description }}</h3>
        <hr class="horizontal-line">
        <h2 :class="getChangePrice">${{ currentPost.price }}</h2>
        <div class="containt">
          <button :class="getChangeColorBuy" class="button-p">Buy Now</button>
          <button :class="getChangeColorNext" class="next-btn" @click="nextPost">Next Product</button>
        </div>
      </div>
    </div>
    <div v-else class="card-2">
      <div class="bg-unknown">
        <img class="image-unknown" src="../../public/unknown.png" alt="" srcset="">
      </div>
      <div class="content-unknown">
        <h2>This Product is unavailable to show</h2>
        <button class="btn-unknown" @click="nextPost">Next Product</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue';

const posts = ref([]);
const currentPostIndex = ref(0);

const fetchData = async () => {
  try {
    const response = await fetch('https://fakestoreapi.com/products');
    const data = await response.json();
    posts.value = data;
  } catch (error) {
    console.error('Error fetching data:', error);
  }
};

onMounted(() => {
  fetchData();
});

const shouldShowCard = computed(() => {
  return (
    currentPost.value &&
    (currentPost.value.category === "men's clothing" ||
      currentPost.value.category === "women's clothing")
  );
});

const currentPost = computed(() => {
  return posts.value[currentPostIndex.value];
});

const nextPost = () => {
  currentPostIndex.value = (currentPostIndex.value + 1) % posts.value.length;
};

const getChangeColorTitle = computed(() => {
  return currentPost.value.category === "women's clothing" ? 'pink-title' : 'normal-title';
});

const getChangeColorBuy = computed(() => {
  return currentPost.value.category === "women's clothing" ? 'pink-buy-btn' : 'button-p';
});

const getChangeColorNext = computed(() => {
  return currentPost.value.category === "women's clothing" ? 'pink-buy-btn-next' : 'next-btn';
});

const getChangeBackground = computed(() => {
  if(currentPost.value.category === "women's clothing"){
    return 'container-1'
  } else if (currentPost.value.category === "men's clothing"){
    return 'container-2'
  } else{
    return 'container-3'
  }
  // return currentPost.value.category === "women's clothing" ? 'container-1' : 'container-2';
});

const getChangePrice = computed(() => {
  return currentPost.value.category === "women's clothing" ? 'price-women' : 'price-men';
});
</script>

<style scoped>
.bg-unknown{
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  position: absolute;
}
.image-unknown{
  padding-top: 10%;
  width: 80%;
  height: 80%;
}

.content-unknown{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: absolute;
  width: 100%;
  height: 100%;
  /* background-color: black; */
}

.btn-unknown{
  width: 420px;
  height: 40px;
  margin-block: 20px;
  border-radius: 5px;
  border: 2px solid black;
  font-size: 17px;
  cursor: pointer;
}

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

.price-women{
  font-size: 26px;
  font-weight: bold;
  margin-top: 5px;
  color: var(--primary-color-women);
}

.price-men{
  font-size: 26px;
  margin-top: 5px;
  font-weight: bold;
  color: var(--primary-color-men);
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
    position: absolute;
    bottom:90px;
    box-shadow: 0px 10px 10px rgba(0, 0, 0, 0.1);
}
.card-2 {
    /* display: none; */
    align-items: center;
    border-radius: 20px;
    background-color: white;
    width: 1053px;
    height: 580px;
    position: absolute;
    bottom: 90px;
    box-shadow: 0px 10px 10px rgba(0, 0, 0, 0.1);
}

.container-1{
  background-color: var(--bg-women);
  display: flex;
  justify-content: center;
  align-items: center;
  height: 487px;
}
.container-2{
  background-color: var(--bg-men);
  display: flex;
  justify-content: center;
  align-items: center;
  height: 487px;
}
.container-3{
  background-color: var(--bg-unknown);
  display: flex;
  justify-content: center;
  align-items: center;
  height: 487px;
}

</style>