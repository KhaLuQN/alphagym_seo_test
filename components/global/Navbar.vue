<template>
  <nav class="navbar bg-black/95 fixed top-0 z-50 border-b border-red-600/30 py-3">
    <div class="navbar-start flex items-center">
      <!-- Dropdown menu for mobile (hamburger icon) -->
      <div class="dropdown lg:hidden">
        <label tabindex="0" class="btn btn-ghost text-red-500 hover:bg-red-600/20 rounded-lg p-2">
          <i class="fas fa-bars text-xl"></i>
        </label>
        <ul
          tabindex="0"
          class="menu menu-sm dropdown-content mt-3 z-[1] p-2 shadow-lg bg-black/95 rounded-box w-52 border border-red-700/50"
        >
          <!-- Mobile navigation links -->
          <li>
            <NuxtLink to="/" class="text-white hover:text-red-500 py-2 px-4 block rounded-md transition-colors duration-200"
              >Trang chủ</NuxtLink
            >
          </li>
          <li>
            <NuxtLink to="/goi-tap/goi-tap" class="text-white hover:text-red-500 py-2 px-4 block rounded-md transition-colors duration-200"
              >Gói tập</NuxtLink
            >
          </li>
          <li>
            <NuxtLink
              to="/huan-luyen-vien"
              class="text-white hover:text-red-500 py-2 px-4 block rounded-md transition-colors duration-200"
              >Huấn luyện viên</NuxtLink
            >
          </li>
          <li>
            <NuxtLink to="/tin-tuc/tin-tuc" class="text-white hover:text-red-500 py-2 px-4 block rounded-md transition-colors duration-200"
              >Tin tức</NuxtLink
            >
          </li>
          <li>
            <NuxtLink to="/lien-he" class="text-white hover:text-red-500 py-2 px-4 block rounded-md transition-colors duration-200"
              >Liên hệ</NuxtLink
            >
          </li>
        </ul>
      </div>

      <!-- Logo -->
      <NuxtLink
        to="/"
        class="btn btn-ghost normal-case text-3xl font-extrabold text-gradient ml-2 lg:ml-0"
      >
        <i class="fas fa-dumbbell mr-2 text-red-500"></i>AlphaGym
      </NuxtLink>
    </div>

    <!-- Center section: Desktop navigation menu -->
    <div class="navbar-center hidden lg:flex">
      <ul class="menu menu-horizontal px-1 space-x-4">
        <li>
          <NuxtLink to="/" class="text-white hover:text-red-500 font-semibold text-lg py-2 px-4 rounded-md transition-colors duration-200"
            >Trang chủ</NuxtLink
          >
        </li>
        <li>
          <NuxtLink
            to="/goi-tap/goi-tap"
            class="text-white hover:text-red-500 font-semibold text-lg py-2 px-4 rounded-md transition-colors duration-200"
            >Gói tập</NuxtLink
          >
        </li>
        <li>
          <NuxtLink
            to="/huan-luyen-vien"
            class="text-white hover:text-red-500 font-semibold text-lg py-2 px-4 rounded-md transition-colors duration-200"
            >Huấn luyện viên</NuxtLink
          >
        </li>
        <li>
          <NuxtLink
            to="/tin-tuc/tin-tuc"
            class="text-white hover:text-red-500 font-semibold text-lg py-2 px-4 rounded-md transition-colors duration-200"
            >Tin tức</NuxtLink
          >
        </li>
        <li>
          <NuxtLink
            to="/lien-he"
            class="text-white hover:text-red-500 font-semibold text-lg py-2 px-4 rounded-md transition-colors duration-200"
            >Liên hệ</NuxtLink
          >
        </li>
      </ul>
    </div>

    <!-- End section: Register button -->
    <div class="navbar-end">
      <NuxtLink to="/dang-ky/dang-ky" class="btn btn-primary bg-red-600 hover:bg-red-700 border-red-600 hover:border-red-700 text-white font-bold text-lg px-6 py-2 rounded-full shadow-lg transform transition-transform duration-200 hover:scale-105">
        Đăng ký ngay
      </NuxtLink>
    </div>
  </nav>
</template>

<style scoped>
/* Gradient text for the logo */
.text-gradient {
  background: linear-gradient(to right, #ff0000, #ff7f00); /* Red to Orange */
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
</style>
