<script>
import axios from "axios";

export default {
  name: "Category",
  data() {
    return {
      categories: [],
    };
  },
  async mounted() {
    try {
      // Ambil data kategori
      const categoryResponse = await axios.get("http://localhost:5000/api/category/categories");
      this.categories = categoryResponse.data;
    } catch (error) {
      console.error("Error fetching categories:", error);
    }
  },
};
</script>

<template>
  <section class="bg-white py-8 antialiased dark:bg-gray-900 md:py-16">
    <div class="mx-auto max-w-screen-xl px-4 2xl:px-0">
      <div class="mb-4 flex items-center justify-between gap-4 md:mb-8">
        <h2 class="text-xl font-semibold text-gray-900 dark:text-white sm:text-2xl">
          Kategori Pilihan
        </h2>

        <router-link
          to="/category"
          class="flex items-center text-base font-medium text-primary-700 hover:underline dark:text-primary-500"
        >
          Lihat Semua
          <svg
            class="ms-1 h-5 w-5"
            aria-hidden="true"
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            fill="none"
            viewBox="0 0 24 24"
          >
            <path
              stroke="currentColor"
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M19 12H5m14 0-4 4m4-4-4-4"
            />
          </svg>
        </router-link>
      </div>

      <div class="grid gap-4 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4">
        <!-- Tampilkan data kategori -->
        <div
          v-for="category in categories"
          :key="category._id"
          class="flex items-center rounded-lg border border-gray-200 bg-white px-4 py-2 hover:bg-gray-50 dark:border-gray-700 dark:bg-gray-800 dark:hover:bg-gray-700"
        >
          <svg
            class="me-2 h-4 w-4 shrink-0 text-gray-900 dark:text-white"
            aria-hidden="true"
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            fill="none"
            viewBox="0 0 24 24"
          >
            <path
              stroke="currentColor"
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M12 15v5m-3 0h6M4 11h16M5 15h14a1 1 0 0 0 1-1V5a1 1 0 0 0-1-1H5a1 1 0 0 0-1 1v9a1 1 0 0 0 1 1Z"
            ></path>
          </svg>
          <span class="text-sm font-medium text-gray-900 dark:text-white">
            {{ category.name }}
          </span>
        </div>
      </div>
    </div>
  </section>
</template>
