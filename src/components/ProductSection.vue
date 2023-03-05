<script setup>
import "../assets/styles/style.css";
import { onMounted, reactive } from "vue";
import UnavailableProduct from "./UnavailableProduct.vue";
import LoadingComponent from "./LoadingComponent.vue";

const state = reactive({
  index: 1,
  isLoading: true,
  category: null,
  data: {},
});

async function getData() {
  state.isLoading = true;
  try {
    const response = await fetch(
      `https://fakestoreapi.com/products/${state.index}`
    );
    const data = await response.json();
    state.data = data;
    if (state.data.category == "men's clothing" || state.data.category == "women's clothing") {
      state.category = state.data.category;
    } else {
      state.category = null;
    }
    console.log(state.data);
    console.log(state.category)
  } catch (error) {
    console.log(error);
  }
  state.isLoading = false;
}
function count() {
  if (state.index < 20) {
    state.index++;
  } else if (state.index === 20) {
    state.index = 1;
  }
  getData();
}

console.log(state.index);

onMounted(() => {
  getData();
});
</script>

<template>
  <main :class="
    state.category ? state.category == 'men\'s clothing' ? 'men' : 'women'
      : 'unavailable'
  ">
    <!-- background -->
    <div class="background">
      <img src="./../assets/bg-pattern.svg" alt="image" />
    </div>

    <!-- card -->
    <div class="card">
      <!-- image product -->
      <div class="card__image" v-if="!state.isLoading && state.category">
        <img :src="state.data.image" alt="image" />
      </div>
      <!-- detail product -->
      <div class="card__detail" v-if="!state.isLoading && state.category">
        <div class="detail__header">
          <!-- title -->
          <h1 class="title">
            {{ state.data.title }}
          </h1>
          <!-- rating -->
          <div class="rate">
            <h2 class="category">{{ state.data.category ?? "" }}</h2>
            <div class="item-rate">
              <p>{{ state.data.rating?.rate }}/5</p>
              <div class="item">
                <div class="item-rate__circle" v-for="item in new Array(Math.round(state.data.rating.rate))" :key="item">
                </div>
              </div>
              <div class="item-rate__unfilled" v-for="item in new Array(
                5 - Math.round(state.data.rating.rate)
              )" :key="item"></div>
            </div>
          </div>
          <!-- line -->
          <hr />
          <!-- description -->
          <p class="description">{{ state.data.description }}</p>
        </div>
        <div class="card__footer">
          <!-- line -->
          <hr />
          <!-- price -->
          <p class="price">${{ state.data.price }}</p>
          <!-- button -->
          <div class="button">
            <button class="btn-primary">Buy Now</button>
            <button class="btn-secondary" @click="count">Next Product</button>
          </div>
        </div>
      </div>
    </div>
    <UnavailableProduct @onNext="count" v-if="!state.isLoading && !state.category" />
    <LoadingComponent v-if="state.isLoading" />
  </main>
</template>
