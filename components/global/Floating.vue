<template>
  <div class="fixed bottom-6 right-6 z-50">
    <div class="dropdown dropdown-top dropdown-end">
      <label tabindex="0" class="btn btn-error btn-circle btn-lg pulse-red">
        <i class="fas fa-comments text-xl"></i>
      </label>
      <div
        tabindex="0"
        class="dropdown-content z-[1] menu p-2 shadow bg-black/95 rounded-box w-52 border border-red-600/30"
      >
        <li>
          <a href="tel:0123456789" class="text-white hover:text-red-400">
            <i class="fas fa-phone"></i> Gọi ngay
          </a>
        </li>
        <li>
          <a href="#" class="text-white hover:text-red-400">
            <i class="fab fa-facebook-messenger"></i> Messenger
          </a>
        </li>
        <li>
          <a href="#" class="text-white hover:text-red-400">
            <i class="fab fa-telegram"></i> Telegram
          </a>
        </li>
        <li>
          <a href="#" class="text-white hover:text-red-400">
            <i class="fas fa-map-marker-alt"></i> Chỉ đường
          </a>
        </li>
      </div>
    </div>
  </div>
</template>
