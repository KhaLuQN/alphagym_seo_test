<template>
  <section id="packages" class="py-20 bg-black">
    <div class="container mx-auto px-4">
      <div class="text-center mb-16">
        <h2 class="text-5xl font-black mb-4 text-gradient">GÓI TẬP POWERGYM</h2>
        <p class="text-xl text-gray-300">
          Lựa chọn gói tập phù hợp với nhu cầu của bạn
        </p>
      </div>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
        <!-- Package 1 -->
        <div class="card bg-gray-900 border border-red-600/30 card-hover">
          <div class="card-body text-center">
            <div class="badge badge-error badge-lg mb-4">KHUYẾN MÃI</div>
            <h3
              class="card-title text-2xl font-bold text-white justify-center mb-4"
            >
              GÓI 1 THÁNG
            </h3>
            <div class="text-center mb-6">
              <span class="text-4xl font-black text-red-500">30 NGÀY</span>
            </div>
            <div class="mb-6">
              <div class="text-lg text-gray-400 line-through">1.000.000đ</div>
              <div class="text-3xl font-black text-white">900.000đ</div>
              <div class="text-sm text-red-400 font-semibold">Giảm 10%</div>
            </div>
            <ul class="text-left space-y-2 mb-6">
              <li class="flex items-center">
                <i class="fas fa-check text-red-500 mr-2"></i>Tập không giới hạn
              </li>
              <li class="flex items-center">
                <i class="fas fa-check text-red-500 mr-2"></i>Tư vấn chế độ dinh
                dưỡng
              </li>
              <li class="flex items-center">
                <i class="fas fa-check text-red-500 mr-2"></i>Sử dụng tất cả
                thiết bị
              </li>
            </ul>
            <button class="btn btn-error btn-block text-white font-bold">
              ĐĂNG KÝ NGAY
            </button>
          </div>
        </div>

        <!-- Package 2 -->
        <div
          class="card bg-gradient-to-b from-red-600 to-red-800 border-2 border-red-400 card-hover relative"
        >
          <div class="absolute -top-4 left-1/2 transform -translate-x-1/2">
            <div class="badge badge-warning badge-lg font-bold">PHỔ BIẾN</div>
          </div>
          <div class="card-body text-center">
            <h3
              class="card-title text-2xl font-bold text-white justify-center mb-4"
            >
              GÓI 3 THÁNG
            </h3>
            <div class="text-center mb-6">
              <span class="text-4xl font-black text-white">90 NGÀY</span>
            </div>
            <div class="mb-6">
              <div class="text-lg text-red-200 line-through">2.700.000đ</div>
              <div class="text-3xl font-black text-white">2.400.000đ</div>
              <div class="text-sm text-yellow-300 font-semibold">
                Tiết kiệm 300.000đ
              </div>
            </div>
            <ul class="text-left space-y-2 mb-6">
              <li class="flex items-center">
                <i class="fas fa-check text-white mr-2"></i>Tập không giới hạn
              </li>
              <li class="flex items-center">
                <i class="fas fa-check text-white mr-2"></i>PT cá nhân 4 buổi
              </li>
              <li class="flex items-center">
                <i class="fas fa-check text-white mr-2"></i>Chế độ dinh dưỡng
                chi tiết
              </li>
              <li class="flex items-center">
                <i class="fas fa-check text-white mr-2"></i>Đánh giá thể trạng
              </li>
            </ul>
            <button class="btn btn-warning btn-block text-black font-bold">
              ĐĂNG KÝ NGAY
            </button>
          </div>
        </div>

        <!-- Package 3 -->
        <div class="card bg-gray-900 border border-red-600/30 card-hover">
          <div class="card-body text-center">
            <div class="badge badge-success badge-lg mb-4">TIẾT KIỆM</div>
            <h3
              class="card-title text-2xl font-bold text-white justify-center mb-4"
            >
              GÓI 6 THÁNG
            </h3>
            <div class="text-center mb-6">
              <span class="text-4xl font-black text-red-500">180 NGÀY</span>
            </div>
            <div class="mb-6">
              <div class="text-lg text-gray-400 line-through">5.400.000đ</div>
              <div class="text-3xl font-black text-white">4.500.000đ</div>
              <div class="text-sm text-green-400 font-semibold">
                Tiết kiệm 900.000đ
              </div>
            </div>
            <ul class="text-left space-y-2 mb-6">
              <li class="flex items-center">
                <i class="fas fa-check text-red-500 mr-2"></i>Tập không giới hạn
              </li>
              <li class="flex items-center">
                <i class="fas fa-check text-red-500 mr-2"></i>PT cá nhân 12 buổi
              </li>
              <li class="flex items-center">
                <i class="fas fa-check text-red-500 mr-2"></i>Massage thư giãn
              </li>
              <li class="flex items-center">
                <i class="fas fa-check text-red-500 mr-2"></i>Tặng áo gym
              </li>
            </ul>
            <button class="btn btn-error btn-block text-white font-bold">
              ĐĂNG KÝ NGAY
            </button>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
