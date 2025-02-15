<script>
import axios from "axios";
import Navbar from "../../components/Navbar.vue";
import NavbarA from "../../components/NavbarA.vue";
import Footer from "../../components/Footer.vue";

export default {
  name: "Cart",
  data() {
    return {
      products: [],
      filteredProducts: [],
      searchTerm: "",
      cartItems: [],
      orders: []
    };
  },
  methods: {
    formatRupiah(number) {
      return new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR' }).format(number).replace("IDR", "").trim();
    },
    handleSearch() {
      this.filteredProducts = this.products.filter(product => {
        const searchLower = this.searchTerm.toLowerCase();
        return (
          product.name.toLowerCase().includes(searchLower) ||
          product.category.name.toLowerCase().includes(searchLower)
        );
      });
    },
    async fetchOrders() {
      try {
        const response = await axios.get("http://localhost:5000/api/orders/:id"); // Ganti dengan endpoint yang sesuai
        this.orders = response.data;
      } catch (error) {
        console.error("Error fetching orders:", error);
      }
    },
    async logout() {
      try {
        await axios.post("http://localhost:5000/api/users/logout", {}, { withCredentials: true });
        localStorage.removeItem("userInfo");
        this.isLoggedIn = false;
      } catch (error) {
        console.error("Logout failed", error);
      }
    },
  },
  async mounted() {
    try {
      const response = await axios.get("http://localhost:5000/api/products/all-products");
      this.products = response.data;
      this.filteredProducts = response.data;
      await this.fetchOrders(); // Ambil data pesanan saat komponen dimuat
    } catch (error) {
      console.error("Error fetching products:", error);
    }
  },
  isLoggedIn: false,
  computed: {
    navbarComponent() {
      return this.isLoggedIn ? "NavbarA" : "Navbar";
    },
  },
  created() {
    const userInfo = localStorage.getItem("userInfo");
    this.isLoggedIn = !!userInfo;
  },
  components: {
    Navbar,
    NavbarA,
    Footer
  },
};
</script>

<template>
  <component :is="navbarComponent" />
  <section class="bg-white py-8 antialiased dark:bg-gray-900 md:py-16">
    <div class="mx-auto max-w-screen-xl px-4 2xl:px-0">
      <h2 class="text-xl font-semibold text-gray-900 dark:text-white sm:text-2xl">Keranjang Belanja</h2>
      <div class="mt-6 sm:mt-8 md:gap-6 lg:flex lg:items-start xl:gap-8">
        <div class="mx-auto w-full flex-none lg:max-w-2xl xl:max-w-4xl">
          <div class="space-y-6">
            <!-- Card Checkout -->
            <div
              class="rounded-lg border border-gray-200 bg-white p-4 shadow-sm dark:border-gray-700 dark:bg-gray-800 md:p-6">
              <div class="space-y-4 md:flex md:items-center md:justify-between md:gap-6 md:space-y-0">
                <a href="#" class="shrink-0 md:order-1">
                  <img class="h-20 w-20 dark:hidden"
                    src="/Images/Fashion/uniqlo_denim_jacket.jpg" alt="imac image" />
                </a>
                <label for="counter-input" class="sr-only">Choose quantity:</label>
                <div class="flex items-center justify-between md:order-3 md:justify-end">
                  <div class="flex items-center">
                    
                    <input type="text" id="counter-input" data-input-counter
                      class="w-10 shrink-0 border-0 bg-transparent text-center text-sm font-medium text-gray-900 focus:outline-none focus:ring-0 dark:text-white"
                      placeholder="" value="2" required />
                    
                  </div>
                  <div class="text-end md:order-4 md:w-32">
                    <p class="text-base font-bold text-gray-900 dark:text-white">Rp 1.598.000,00</p>
                  </div>
                </div>

                <div class="w-full min-w-0 flex-1 space-y-4 md:order-2 md:max-w-md">
                  <a href="#" class="text-base font-bold text-gray-900 hover:underline dark:text-white">
                    Uniqlo Denim Jacket
                  </a>

                  <div class="flex items-center gap-4">
                    <button type="button"
                      class="inline-flex items-center text-sm font-medium text-gray-500 hover:text-gray-900 hover:underline dark:text-gray-400 dark:hover:text-white">
                      <svg class="me-1.5 h-5 w-5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" width="24"
                        height="24" fill="none" viewBox="0 0 24 24">
                        <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                          d="M12.01 6.001C6.5 1 1 8 5.782 13.001L12.011 20l6.23-7C23 8 17.5 1 12.01 6.002Z" />
                      </svg>
                      Add to Favorites
                    </button>

                    <button type="button"
                      class="inline-flex items-center text-sm font-medium text-red-600 hover:underline dark:text-red-500">
                      <svg class="me-1.5 h-5 w-5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" width="24"
                        height="24" fill="none" viewBox="0 0 24 24">
                        <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                          d="M6 18 17.94 6M18 18 6.06 6" />
                      </svg>
                      Remove
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="mt-8 xl:block">
            <h3 class="text-2xl font-semibold text-gray-900 dark:text-white">Orang lain juga beli</h3>
            <div class="mt-6 grid grid-cols-1 md:grid-cols-3 gap-4 sm:mt-8">
              <!-- Card Product -->
              <div v-for="product in filteredProducts.slice(0, 6)" :key="product._id"
                class="space-y-6 overflow-hidden rounded-lg border border-gray-200 bg-white p-6 shadow-sm dark:border-gray-700 dark:bg-gray-800">
                <a href="#" class="overflow-hidden rounded">
                  <router-link :to="`/detail-product/${product._id}`">
                    <img :src="`/Images/${product.category.name}/${product.image}`" :alt="product.name"
                      class="mx-auto" />
                  </router-link>
                </a>
                <div>
                  <a href="#"
                    class="text-lg font-semibold leading-tight text-gray-900 hover:underline dark:text-white">{{
                    product.name }}</a>
                  <p class="mt-2 text-base font-normal text-gray-500 dark:text-gray-400">{{ product.description }}</p>
                </div>
                <div>
                  <p class="text-lg font-bold text-gray-900 dark:text-white">
                    <span> {{ formatRupiah(product.price) }} </span>
                  </p>
                </div>
                <div class="mt-6 flex items-center gap-2.5">
                  <button data-tooltip-target="favourites-tooltip-1" type="button"
                    class="inline-flex items-center justify-center gap-2 rounded-lg border border-gray-200 bg-white p-2.5 text-sm font-medium text-gray-900 hover:bg-gray-100 hover:text-primary-700 focus:z-10 focus:outline-none focus:ring-4 focus:ring-gray-100 dark:border-gray-600 dark:bg-gray-800 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white dark:focus:ring-gray-700">
                    <svg class="h-5 w-5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none"
                      viewBox="0 0 24 24">
                      <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                        d="M12 6C6.5 1 1 8 5.8 13l6.2 7 6.2-7C23 8 17.5 1 12 6Z"></path>
                    </svg>
                  </button>
                  <div id="favourites-tooltip-1" role="tooltip"
                    class="tooltip invisible absolute z-10 inline-block rounded-lg bg-gray-900 px-3 py-2 text-sm font-medium text-white opacity-0 shadow-sm transition-opacity duration-300 dark:bg-gray-700">
                    Add to favourites
                    <div class="tooltip-arrow" data-popper-arrow></div>
                  </div>
                  <button type="button"
                    class="inline-flex w-full items-center justify-center rounded-lg bg-primary-700 px-5 py-2.5 text-sm font-medium  text-white hover:bg-primary-800 focus:outline-none focus:ring-4 focus:ring-primary-300 dark:bg-primary-600 dark:hover:bg-primary-700 dark:focus:ring-primary-800">
                    <svg class="-ms-2 me-2 h-5 w-5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" width="24"
                      height="24" fill="none" viewBox="0 0 24 24">
                      <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                        d="M5 4h1.5L9 16m0 0h8m-8 0a2 2 0 1 0 0 4 2 2 0 0 0 0-4Zm8 0a2 2 0 1 0 0 4 2 2 0 0 0 0-4Zm-8.5-3h9.25L19 7h-1M8 7h-.688M13 5v4m-2-2h4" />
                    </svg>
                    Add to cart
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Ringkasan Belanja -->
        <div class="mx-auto mt-6 max-w-4xl flex-1 space-y-6 lg:mt-0 lg:w-full">
          <div
            class="space-y-4 rounded-lg border border-gray-200 bg-white p-4 shadow-sm dark:border-gray-700 dark:bg-gray-800 sm:p-6">
            <p class="text-xl font-semibold text-gray-900 dark:text-white">Ringkasan Belanja</p>
            <div class="space-y-4">
              <div class="space-y-2">
                <dl class="flex items-center justify-between gap-4">
                  <dt class="text-base font-normal text-gray-500 dark:text-gray-400">Total Item</dt>
                  <dd class="text-base font-bold text-gray-900 dark:text-white">
                    2
                  </dd>
                </dl>
                <dl class="flex items-center justify-between gap-4">
                  <dt class="text-base font-normal text-gray-500 dark:text-gray-400">Subtotal</dt>
                  <dd class="text-base font-bold text-gray-900 dark:text-white">
                    Rp. 1.598.000,00
                  </dd>
                </dl>
                <dl class="flex items-center justify-between gap-4">
                  <dt class="text-base font-normal text-gray-500 dark:text-gray-400">Pajak</dt>
                  <dd class="text-base font-bold text-gray-900 dark:text-white">
                    Rp.15.980,00
                  </dd>
                </dl>
                <dl class="flex items-center justify-between gap-4">
                  <dt class="text-base font-normal text-gray-500 dark:text-gray-400">Ongkir</dt>
                  <dd class="text-base font-bold text-gray-900 dark:text-white">
                    Rp.20.000,00
                  </dd>
                </dl>
              </div>

              <dl class="flex items-center justify-between gap-4 border-t border-gray-200 pt-2 dark:border-gray-700">
                <dt class="text-base font-bold text-gray-900 dark:text-white">Total</dt>
                <dd class="text-base font-bold text-gray-900 dark:text-white">
                  Rp. 1.633.980,00
                </dd>
              </dl>
            </div>

            <router-link to="/order"
              class="flex w-full items-center justify-center rounded-lg bg-primary-700 px-5 py-2.5 text-sm font-medium text-white hover:bg-primary-800 focus:outline-none focus:ring-4 focus:ring-primary-300 dark:bg-primary-600 dark:hover:bg-primary-700 dark:focus:ring-primary-800">
              Checkout
            </router-link>

            <div class="flex items-center justify-center gap-2">
              <span class="text-sm font-normal text-gray-500 dark:text-gray-400"> atau </span>
              <router-link to="/product"
                class="inline-flex items-center gap-2 text-sm font-medium text-primary-700 underline hover:no-underline dark:text-primary-500">
                lanjut berbelanja
              </router-link>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
  <Footer />
</template>