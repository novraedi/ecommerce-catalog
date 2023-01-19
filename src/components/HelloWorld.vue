<template>
  <div>
    <div
      :class="[
        product.category === 'men\'s clothing'
          ? 'bg-pattern-men'
          : product.category === 'women\'s clothing'
          ? 'bg-pattern-women'
          : 'bg-unavailable',
      ]"
    >
      <div class="container">
        <div v-if="loading" class="card-skeleton">
          <div class="card-skeleton__container">
            <div class="card-skeleton__image"></div>
            <div class="card-skeleton__content">
              <div class="card-skeleton__title"></div>
              <div class="card-skeleton__rating"></div>
              <div class="card-skeleton__desc"></div>
              <div class="card-skeleton__desc"></div>
              <div class="card-skeleton__desc space"></div>
              <div class="card-skeleton__price"></div>
              <div class="card-skeleton__buttoncol">
                <div class="card-skeleton__button"></div>
                <div class="card-skeleton__button"></div>
              </div>
            </div>
          </div>
        </div>
        <div
          v-else
          class="card"
          :class="[
            product.category === 'men\'s clothing'
              ? 'theme-men'
              : product.category === 'women\'s clothing'
              ? 'theme-women'
              : 'theme-unavailable',
          ]"
        >
          <div
            v-if="
              product.category === 'men\'s clothing' ||
              product.category === 'women\'s clothing'
            "
          >
            <div class="container-card">
              <div class="image-product">
                <img :src="product.image" alt="" height="300" />
              </div>
              <div class="desc-product">
                <div class="title-product">
                  <h2>{{ product.title }}</h2>
                </div>
                <div class="rating">
                  <span>{{ product.category }}</span>
                  <div class="container-rate">
                    <span>{{ `${product.rating?.rate}/5` }}</span>
                    <div v-for="i in 5" :key="i">
                      <i v-if="i <= rating" class="for-rate active"></i>
                      <i v-else class="for-rate"></i>
                    </div>
                  </div>
                </div>
                <hr />
                <div class="desc">
                  <p>{{ product.description }}</p>
                </div>
                <hr />
                <div class="section-price">
                  <h2 class="price">{{ "$" + product.price }}</h2>
                  <button class="btn">Buy Now</button>
                  <button
                    @click="fetchData()"
                    class="btn-reverse"
                    :style="'margin-left:20px'"
                  >
                    Next Product
                  </button>
                </div>
              </div>
            </div>
          </div>
          <div v-else>
            <div class="container-card">
              <div class="unavailable">
                <div class="bg-sad">
                  <h2>This product is unavailable to show</h2>
                  <button
                    @click="fetchData()"
                    class="btn-reverse"
                    :style="'margin-left:20px'"
                  >
                    Next Product
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      index: 1,
      product: {},
      rating: 0,
      loading: true,
    };
  },
  created() {
    setTimeout(() => {
      this.fetchData();
    }, 1000);
  },
  methods: {
    async fetchData() {
      await axios
        .get("https://fakestoreapi.com/products/" + this.index)
        .then((response) => {
          console.log(response.data);
          this.product = response.data;
          this.loading = false;

          //Pembulatan rating
          this.rating = Math.round(response.data.rating.rate);
          this.index++;
          if (this.index > 20) {
            this.index = 1;
          }
        })
        .catch((error) => {
          console.log(error);
        });
    },
    async getData() {
      await axios
        .get("https://fakestoreapi.com/products")
        .then((response) => {
          console.log(response.data);
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>
<style></style>
