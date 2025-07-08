<template>
  <nav class="navbar bg-black/95 fixed top-0 z-50 border-b border-red-600/30">
    <div class="navbar-start">
      <div class="dropdown">
        <label tabindex="0" class="btn btn-ghost lg:hidden">
          <i class="fas fa-bars text-red-500"></i>
        </label>
        <ul
          class="menu menu-sm dropdown-content mt-3 z-[1] p-2 shadow bg-black/95 rounded-box w-52"
        >
          <li>
            <NuxtLink to="/" class="text-white hover:text-red-500"
              >Trang chủ</NuxtLink
            >
          </li>
          <li>
            <NuxtLink to="/goi-tap" class="text-white hover:text-red-500"
              >Gói tập</NuxtLink
            >
          </li>
          <li>
            <NuxtLink
              to="/huan-luyen-vien"
              class="text-white hover:text-red-500"
              >Huấn luyện viên</NuxtLink
            >
          </li>
          <li>
            <NuxtLink to="/tin-tuc" class="text-white hover:text-red-500"
              >Tin tức</NuxtLink
            >
          </li>
          <li>
            <NuxtLink to="/lien-he" class="text-white hover:text-red-500"
              >Liên hệ</NuxtLink
            >
          </li>
        </ul>
      </div>
      <NuxtLink
        to="/"
        class="btn btn-ghost normal-case text-2xl font-bold text-gradient"
      >
        <i class="fas fa-dumbbell mr-2"></i>AlphaGym
      </NuxtLink>
    </div>

    <div class="navbar-center hidden lg:flex">
      <ul class="menu menu-horizontal px-1">
        <li>
          <NuxtLink to="/" class="text-white hover:text-red-500 font-semibold"
            >Trang chủ</NuxtLink
          >
        </li>
        <li>
          <NuxtLink
            to="/goi-tap"
            class="text-white hover:text-red-500 font-semibold"
            >Gói tập</NuxtLink
          >
        </li>
        <li>
          <NuxtLink
            to="/huan-luyen-vien"
            class="text-white hover:text-red-500 font-semibold"
            >Huấn luyện viên</NuxtLink
          >
        </li>
        <li>
          <NuxtLink
            to="/tin-tuc"
            class="text-white hover:text-red-500 font-semibold"
            >Tin tức</NuxtLink
          >
        </li>
        <li>
          <NuxtLink
            to="/lien-he"
            class="text-white hover:text-red-500 font-semibold"
            >Liên hệ</NuxtLink
          >
        </li>
      </ul>
    </div>

    <div class="navbar-end">
      <NuxtLink
        to="/dang-ky"
        class="btn btn-error text-white font-bold pulse-red"
      >
        <i class="fas fa-phone mr-2"></i>Đăng ký ngay
      </NuxtLink>
    </div>
  </nav>
</template>
