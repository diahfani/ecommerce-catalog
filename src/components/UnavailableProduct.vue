<script setup>
import { defineEmits } from "vue";

const emit = defineEmits(["onNext"]);

function next() {
  emit("onNext");
}
</script>
<template>
  <div class="unavailable-product">
    <p>This product is unavailable to show</p>
    <button class="btn-unavail" @click="next">Next Product</button>
  </div>
</template>
