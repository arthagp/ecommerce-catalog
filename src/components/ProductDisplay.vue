<template>
  <div v-if="isLoading" class="loading-container">
    <div class="loading-spinner"></div>
  </div>

  <div class="bg" :class="product.category === `men's clothing`
    ? 'bg-men'
    : product.category === `women's clothing`
      ? 'bg-women'
      : 'bg-unknown'
    ">
    <div v-if="product.category === `men's clothing` ||
      product.category === `women's clothing`
      " class="card">
      <div class="product-img">
        <img :src="product.image" alt="product-image" width="301" />
      </div>
      <div class="product-desc">
        <h1 class="title" :class="product.category === `men's clothing`
          ? 'text-dark-blue'
          : 'text-dark-purple'
          ">
          {{ product.title }}
        </h1>
        <div class="category-rating">
          <p>{{ product.category }}</p>
          <div class="rating">
            <span class="rate">{{ product.rating.rate }}/5</span>
            <div v-if="product.rating && product.rating.rate" class="rating-circles">
              <div v-for="rate in Math.floor(product.rating.rate)" :key="rate" :class="product.category === `men's clothing`
                ? 'circle-men'
                : 'circle-women'
                "></div>
            </div>
          </div>
        </div>
        <hr class="horizontal-line" />

        <p class="description line-clamp">{{ product.description }}</p>
        <hr class="horizontal-line" />
        <h1 class="price" :class="product.category === `men's clothing` ? 'text-dark-blue' : 'text-dark-purple'
          ">
          ${{ product.price }}
        </h1>
        <div class="btn-group">
          <button class="btn" :class="product.category === `men's clothing`
            ? 'btn-buy-men'
            : 'btn-buy-women'
            ">
            Buy now
          </button>
          <button @click="getProduct" class="btn" :class="product.category === `men's clothing`
            ? 'btn-next-men'
            : 'btn-next-women'
            ">
            Next product
          </button>
        </div>
      </div>
    </div>
    <div v-else class="card card-unknown">
      <p class="txt-unkknown">This product is unavailable to show</p>
      <button @click="getProduct" class="btn btn-next-unkknown">
        Next product
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductDisplay",
  data() {
    return {
      index: 0,
      product: {},
      rating: [1, 2, 3, 4, 5],
      isLoading: false,
    };
  },
  methods: {
    async getProduct() {
      this.isLoading = true;
      this.index >= 20 ? (this.index = 1) : this.index++;

      const response = await fetch(
        `https://fakestoreapi.com/products/${this.index}`
      );
      const data = await response.json();

      this.product = data;
      this.isLoading = false;
    },
  },
  mounted() {
    this.getProduct();
  },
};
</script>

<style>
@import '../assets/style/page.css';
</style>


