<template>
  <div
    id="productCard"
    class="rounded-lg shadow-md m-4 w-1/2 border bg-gray-200 border-gray-500"
  >
    <div id="content" class="flex p-2">
      <img
        class="w-1/2 rounded-lg border-gray-400 border shadow-md object-center"
        :src="getImage"
        alt="Product"
      />
      <div class="pl-6 w-1/2">
        <h3 class="text-xl font-bold">{{ name }}</h3>

        <h2 class="font-bold mt-3">Description</h2>
        <p class="tracking-tight">{{ description }}</p>

        <p class="mt-3">
          Total items in the shop:
          <b class="font-bold">
            {{ getInventory }}
          </b>
        </p>

        <div class="mt-3">
          <p v-if="getInventory > 10">In stock</p>
          <p v-else-if="getInventory >= 1">Almost out of stock</p>
          <p v-else>Out stock</p>
        </div>

        <h2 class="font-bold mt-3">Specs:</h2>
        <ul class="tracking-tight" v-for="detail in details" :key="detail">
          <li class="list-disc ml-4">{{ detail }}</li>
        </ul>

        <h3 class="mt-3 font-bold">Colors:</h3>
        <div class="flex flex-columns">
          <p
            v-for="variant in variants"
            :key="variant.index"
            :class="getCss(variant.index)"
            @mouseover="changeColor(variant.index)"
          >
            {{ variant.color }}
          </p>
        </div>

        <button
          class="rounded shadow hover:bg-teal-500 float-right mr-2 mt-3 border border-teal-700 bg-teal-300 px-4 py-2"
          :class="{
            'opacity-50 cursor-not-allowed': getInventory == 0,
          }"
          @click="addToCart"
          :disabled="getInventory == 0"
        >
          Add to cart
        </button>
        <button
          class="rounded shadow hover:bg-teal-500 float-right mr-2 mt-3 border border-teal-700 bg-teal-300 px-4 py-2"
          :class="{
            'opacity-50 cursor-not-allowed': allowRemove,
          }"
          @click="removeToCard"
          :disabled="allowRemove"
        >
          Remove from cart
        </button>
      </div>
    </div>

    <div>
      <p class="text-center text-xl underline m-2" v-if="reviews.length == 0">
        No reviews yet
      </p>
      <div class="m-2" v-else>
        <h3 class="mb-3 text-center text-xl underline">Reviews</h3>
        <ul class="flex">
          <li
            class="py-1 px-4 w-1/4 border-2 rounded border-gray-300 bg-white"
            v-for="review in reviews"
            :key="review.review"
          >
            <span>{{ review.user }}</span>
            <span class="text-orange-500 ml-2">{{
              getStars(review.rate)
            }}</span>
            <p class="italic text-sm">"{{ review.review }}"</p>
          </li>
        </ul>
      </div>
    </div>

    <RateForm @handleSubmitRate="handleSubmitRate" />
  </div>
</template>

<script>
import RateForm from "@/components/RateForm";

export default {
  name: "ProductCard",
  components: { RateForm },
  props: ["id", "name", "description", "details", "variants"],
  data: () => ({
    index: 0,
    reviews: [],
  }),
  methods: {
    addToCart() {
      this.$emit("handleCart", "add", this.id, this.index);
    },
    removeToCard() {
      this.$emit("handleCart", "remove", this.id, this.index);
    },
    changeColor(index) {
      this.index = index;
    },
    getCss(index) {
      const addCss = this.variants.find((x) => x.index == index).additionalCss;
      return `mr-2 border-2 rounded-full px-2 py-1 text-white ${addCss}`;
    },
    handleSubmitRate(review) {
      this.reviews.push(review);
    },
    getStars(rate) {
      let stars = "";
      for (let x = 0; x < rate; x++) stars += "★";

      return stars;
    },
  },
  computed: {
    getImage() {
      return this.variants.find((x) => x.index == this.index).src;
    },
    getInventory() {
      return this.variants.find((x) => x.index == this.index).inventory;
    },
    allowRemove() {
      return this.variants.find((x) => x.index == this.index).inCart == 0;
    },
  },
};
</script>

<style scoped >
#content {
  min-height: 32rem;
}
</style>