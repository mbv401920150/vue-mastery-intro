<template>
  <div class="w-full flex items-center justify-center mb-3">
    <div class="flex flex-col justify-center" v-if="!showForm">
      <p>Do you want write a review?</p>
      <button
        class="mx-2 my-1 border border-gray-900 rounded shadow text-white bg-gray-600 hover:bg-gray-700"
        @click="showHideForm"
      >
        Write a review
      </button>
    </div>
    <form
      v-else
      class="w-2/3 my-3 p-3 border border-gray-600 bg-gray-300 shadow-md rounded"
      @submit.prevent="handleSubmit"
    >
      <h1 class="text-2xl text-center underline">Submit a review</h1>

      <div
        class="mb-3 border-2 p-3 rounded border-red-900 bg-red-300 text-red-900"
        v-if="errors.length > 0"
      >
        <p class="text-xl">Please check the following errors:</p>
        <ul class="leading-tight">
          <li class="list-disc ml-5" v-for="error in errors" :key="error">
            {{ error }}
          </li>
        </ul>
      </div>

      <p class="mb-3 flex flex-col">
        <label for="userName">User</label>
        <input
          class="border-2 p-2"
          id="userName"
          type="text"
          v-model="userName"
        />
      </p>

      <p class="mb-3 flex flex-col">
        <label for="userReview">Review</label>
        <textarea class="border-2 p-2" id="userReview" v-model="userReview" />
      </p>

      <p class="mb-3 flex flex-col">
        <label for="userRate">Rate</label>
        <select class="border-2 p-2" id="userRate" v-model.number="userRate">
          <option value="1">1</option>
          <option value="2">2</option>
          <option value="3">3</option>
          <option value="4">4</option>
          <option value="5">5</option>
        </select>
      </p>

      <p class="flex">
        <input
          class="shadow rounded px-4 py-2 bg-gray-400 border border-gray-800 hover:bg-gray-500 focus:bg-gray-500"
          type="submit"
          value="Submit"
        />
        <input
          class="shadow rounded ml-2 px-4 py-2 bg-gray-400 border border-gray-800 hover:bg-gray-500 focus:bg-gray-500"
          type="button"
          value="Cancel"
          @click="showHideForm"
        />
      </p>
    </form>
  </div>
</template>

<script>
export default {
  name: "RateForm",
  data: () => ({
    userName: null,
    userReview: null,
    userRate: null,
    showForm: false,
    errors: [],
  }),
  methods: {
    showHideForm() {
      this.showForm = !this.showForm;
    },

    handleSubmit() {
      this.errors = [];

      // Track errors
      if (!this.userName || !this.userReview || !this.userRate) {
        if (!this.userName) this.errors.push("The user name is required");
        if (!this.userReview) this.errors.push("The user review is required");
        if (!this.userRate) this.errors.push("The user rate is required");
      } else {
        const review = {
          user: this.userName,
          review: this.userReview,
          rate: this.userRate,
        };

        this.userName = null;
        this.userReview = null;
        this.userRate = null;

        this.$emit("handleSubmitRate", review);
      }
    },
  },
};
</script>