<script>
import axios from "axios";

export default {
  methods: {
    async handleLogout() {
      try {
        await axios.post("http://localhost:5000/api/users/logout", {}, { withCredentials: true });
        
        // Emit event logout ke komponen induk
        this.$emit("userLoggedOut");

        // Tambahan: Arahkan pengguna ke halaman login jika perlu
        window.location.href = "/login";
        localStorage.removeItem("userInfo");
      } catch (error) {
        console.error("Error logging out:", error);
        alert("Failed to logout. Please try again.");
      }
    },
  },
  mounted() {
    document.addEventListener("click", (event) => {
      const dropdown = document.getElementById("user-dropdown");
      const button = document.getElementById("user-menu-button");
      if (dropdown && button && !dropdown.contains(event.target) && !button.contains(event.target)) {
        dropdown.classList.add("hidden");
      }
    });

    const button = document.getElementById("user-menu-button");
    button?.addEventListener("click", () => {
      const dropdown = document.getElementById("user-dropdown");
      dropdown?.classList.toggle("hidden");
    });
  },
};
</script>

<template>
  <nav class="border-gray-200 bg-gray-900">
    <div class="mx-auto flex h-16 max-w-screen-xl items-center gap-8 px-4 sm:px-6 lg:px-8">
        <router-link to="/" class="block text-teal-300" href="#">
          <span class="sr-only">Home</span>
          <svg class="h-8" viewBox="0 0 28 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path
              d="M0.41 10.3847C1.14777 7.4194 2.85643 4.7861 5.2639 2.90424C7.6714 1.02234 10.6393 0 13.695 0C16.7507 0 19.7186 1.02234 22.1261 2.90424C24.5336 4.7861 26.2422 7.4194 26.98 10.3847H25.78C23.7557 10.3549 21.7729 10.9599 20.11 12.1147C20.014 12.1842 19.9138 12.2477 19.81 12.3047H19.67C19.5662 12.2477 19.466 12.1842 19.37 12.1147C17.6924 10.9866 15.7166 10.3841 13.695 10.3841C11.6734 10.3841 9.6976 10.9866 8.02 12.1147C7.924 12.1842 7.8238 12.2477 7.72 12.3047H7.58C7.4762 12.2477 7.376 12.1842 7.28 12.1147C5.6171 10.9599 3.6343 10.3549 1.61 10.3847H0.41ZM23.62 16.6547C24.236 16.175 24.9995 15.924 25.78 15.9447H27.39V12.7347H25.78C24.4052 12.7181 23.0619 13.146 21.95 13.9547C21.3243 14.416 20.5674 14.6649 19.79 14.6649C19.0126 14.6649 18.2557 14.416 17.63 13.9547C16.4899 13.1611 15.1341 12.7356 13.745 12.7356C12.3559 12.7356 11.0001 13.1611 9.86 13.9547C9.2343 14.416 8.4774 14.6649 7.7 14.6649C6.9226 14.6649 6.1657 14.416 5.54 13.9547C4.4144 13.1356 3.0518 12.7072 1.66 12.7347H0V15.9447H1.61C2.39051 15.924 3.154 16.175 3.77 16.6547C4.908 17.4489 6.2623 17.8747 7.65 17.8747C9.0377 17.8747 10.392 17.4489 11.53 16.6547C12.1468 16.1765 12.9097 15.9257 13.69 15.9447C14.4708 15.9223 15.2348 16.1735 15.85 16.6547C16.9901 17.4484 18.3459 17.8738 19.735 17.8738C21.1241 17.8738 22.4799 17.4484 23.62 16.6547ZM23.62 22.3947C24.236 21.915 24.9995 21.664 25.78 21.6847H27.39V18.4747H25.78C24.4052 18.4581 23.0619 18.886 21.95 19.6947C21.3243 20.156 20.5674 20.4049 19.79 20.4049C19.0126 20.4049 18.2557 20.156 17.63 19.6947C16.4899 18.9011 15.1341 18.4757 13.745 18.4757C12.3559 18.4757 11.0001 18.9011 9.86 19.6947C9.2343 20.156 8.4774 20.4049 7.7 20.4049C6.9226 20.4049 6.1657 20.156 5.54 19.6947C4.4144 18.8757 3.0518 18.4472 1.66 18.4747H0V21.6847H1.61C2.39051 21.664 3.154 21.915 3.77 22.3947C4.908 23.1889 6.2623 23.6147 7.65 23.6147C9.0377 23.6147 10.392 23.1889 11.53 22.3947C12.1468 21.9165 12.9097 21.6657 13.69 21.6847C14.4708 21.6623 15.2348 21.9135 15.85 22.3947C16.9901 23.1884 18.3459 23.6138 19.735 23.6138C21.1241 23.6138 22.4799 23.1884 23.62 22.3947Z"
              fill="currentColor"
            />
          </svg>
        </router-link>
      <div class="flex items-center md:order-2 space-x-3 md:space-x-0 rtl:space-x-reverse">
        <button type="button"
          class="flex text-sm bg-gray-800 rounded-full md:me-0 focus:ring-4 focus:ring-gray-300 dark:focus:ring-gray-600"
          id="user-menu-button" aria-expanded="false" data-dropdown-toggle="user-dropdown"
          data-dropdown-placement="bottom">
          <span class="sr-only">Open user menu</span>
          <img class="w-8 h-8 rounded-full" src="https://via.placeholder.com/150" alt="user photo">
        </button>
        <!-- Dropdown menu -->
        <div
          class="z-50 hidden my-4 text-base list-none divide-y divide-gray-100 rounded-lg shadow bg-gray-700 dark:divide-gray-600"
          id="user-dropdown" style="top: 100%; left: 0; position: absolute;">
          <div class="px-4 py-3">
            <span class="block text-sm text-white">Bonnie Green</span>
            <span class="block text-sm  text-gray-500 truncate dark:text-gray-400">name@flowbite.com</span>
          </div>
          <ul class="py-2" aria-labelledby="user-menu-button">
            <li>
              <a href="#" @click.prevent="handleLogout"
                class="block px-4 py-2 text-sm hover:bg-gray-600 text-gray-200 dark:hover:text-white">
                Sign out
              </a>
            </li>

          </ul>
        </div>
        <button data-collapse-toggle="navbar-user" type="button"
          class="inline-flex items-center p-2 w-10 h-10 justify-center text-sm text-gray-500 rounded-lg md:hidden hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-gray-200 dark:text-gray-400 dark:hover:bg-gray-700 dark:focus:ring-gray-600"
          aria-controls="navbar-user" aria-expanded="false">
          <span class="sr-only">Open main menu</span>
          <svg class="w-5 h-5" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 17 14">
            <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
              d="M1 1h15M1 7h15M1 13h15" />
          </svg>
        </button>
      </div>
      <div class="flex flex-1 items-center justify-end md:justify-between">
          <nav aria-label="Global" class="hidden md:block">
            <ul class="flex items-center gap-6 text-sm">
              <li>
                <router-link to="/" class="transition text-white hover:text-white/75"> Home </router-link>
              </li>
              <li>
                <router-link to="/about" class="transition text-white hover:text-white/75"> Tentang </router-link>
              </li>
    
              <li>
                <router-link to="/list-article" class="transition text-white hover:text-white/75"> Artikel </router-link>
              </li>
    
              <li>
                <router-link to="/product" class="transition text-white hover:text-white/75"> Produk </router-link>
              </li>
    
              <li>
                <router-link to="/contact" class="transition text-white hover:text-white/75"> Kontak </router-link>
              </li>
              <li>
                <router-link to="/Cart" class="transition text-white hover:text-white/75"> Cart </router-link>
              </li>
            </ul>
          </nav>
        </div>
    </div>
  </nav>

</template>