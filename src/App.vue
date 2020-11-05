<template>
  <div id="app">
    <nav class="flex items-center justify-between flex-wrap bg-teal-400 p-6">
      <h1 class="text-2xl text-white text-bold">Vue Store</h1>

      <div><i class="fas fa-2x fa-shopping-cart"></i> ({{ getCount }})</div>
    </nav>

    <main class="flex">
      <ProductCard
        v-for="product in productData"
        :key="product.id"
        :id="product.id"
        :name="product.name"
        :description="product.description"
        :details="product.details"
        :variants="product.variants"
        @handleCart="handleCart"
      />
    </main>

    <UserForm />
    <pre>{{ JSON.stringify({ productData, cart }, null, 2) }}</pre>
  </div>
</template>

<script>
import data from "./data";
import ProductCard from "./components/ProductCard";

export default {
  name: "App",
  components: { ProductCard },

  data: () => ({
    productData: data,
    cart: [],
  }),
  methods: {
    handleCart(action, id, variantIndex) {
      const product = this.productData.find((x) => x.id === id);
      const variant = product.variants.find((x) => x.index === variantIndex);

      let addItem = false;
      let cartItem = this.cart.find(
        (x) => x.productId == id && x.variant == variantIndex
      );

      if (cartItem == null) {
        addItem = true;
        cartItem = {
          productId: id,
          variant: variantIndex,
          count: 0,
        };
      }

      switch (action) {
        case "add":
          cartItem.count += 1;
          variant.inventory -= 1;
          break;
        case "remove":
          cartItem.count -= 1;
          variant.inventory += 1;

          break;
      }

      variant.inCart = cartItem.count;

      if (addItem) this.cart.push(cartItem);
    },
  },
  computed: {
    getCount() {
      if (this.cart.count === 0) return 0;

      let total = 0;
      this.cart.forEach((x) => (total += x.count));

      return total;
    },
  },
};
</script>

<style scoped>
</style>
