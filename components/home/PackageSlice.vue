<template>
  <section id="packages" class="py-20 bg-gray-950 text-white">
    <div class="container mx-auto px-6 lg:px-8">
      <div class="text-center mb-16">
        <h2 class="text-5xl lg:text-6xl font-extrabold mb-4 text-red-500 custom-text-shadow">GÓI TẬP ALPHAGYM</h2>
        <p class="text-xl text-gray-300 max-w-2xl mx-auto">
          Lựa chọn gói tập phù hợp với nhu cầu và mục tiêu của bạn
        </p>
      </div>
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-10">
        <!-- Package 1: GÓI 1 THÁNG (KHUYẾN MÃI) -->
        <div class="card bg-gray-900 rounded-2xl shadow-xl border border-red-800/30 overflow-hidden transform transition-all duration-300 hover:scale-[1.02] hover:shadow-2xl hover:shadow-red-900/50">
          <div class="card-body p-8 text-center flex flex-col items-center">
            <div class="badge badge-error text-white font-bold text-base px-4 py-2 rounded-full mb-6 shadow-md">KHUYẾN MÃI</div>
            <h3 class="card-title text-3xl font-extrabold text-white mb-4 justify-center">
              GÓI 1 THÁNG
            </h3>
            <div class="text-center mb-6">
              <span class="text-5xl font-black text-red-500 block">30 NGÀY</span>
            </div>
            <div class="mb-8 w-full">
              <div class="text-lg text-gray-400 line-through mb-1">1.000.000đ</div>
              <div class="text-4xl font-black text-white">900.000đ</div>
              <div class="text-sm text-red-400 font-semibold mt-1">Giảm 10%</div>
            </div>
            <ul class="text-left space-y-3 mb-8 w-full text-gray-300">
              <li class="flex items-center">
                <i class="fas fa-check text-red-500 mr-3 text-lg"></i>Tập không giới hạn
              </li>
              <li class="flex items-center">
                <i class="fas fa-check text-red-500 mr-3 text-lg"></i>Tư vấn chế độ dinh dưỡng
              </li>
              <li class="flex items-center">
                <i class="fas fa-check text-red-500 mr-3 text-lg"></i>Sử dụng tất cả thiết bị
              </li>
            </ul>
            <button class="btn btn-error btn-block text-white font-bold text-lg py-3 rounded-full shadow-lg hover:bg-red-700 transform transition-transform duration-200 hover:scale-105">
              ĐĂNG KÝ NGAY
            </button>
          </div>
        </div>

        <!-- Package 2: GÓI 3 THÁNG (PHỔ BIẾN) -->
        <div class="card bg-gradient-to-br from-red-700 to-red-900 rounded-2xl shadow-2xl border-2 border-red-400 overflow-hidden transform transition-all duration-300 hover:scale-[1.03] hover:shadow-red-500/50 relative">
          <div class="absolute -top-8 left-1/2 transform -translate-x-1/2 z-10">
            <div class="badge badge-warning text-black font-bold text-base px-5 py-2 rounded-full shadow-lg border border-yellow-400">PHỔ BIẾN</div>
          </div>
          <div class="card-body p-8 text-center flex flex-col items-center pt-10">
            <h3 class="card-title text-3xl font-extrabold text-white mb-4 justify-center">
              GÓI 3 THÁNG
            </h3>
            <div class="text-center mb-6">
              <span class="text-5xl font-black text-white block">90 NGÀY</span>
            </div>
            <div class="mb-8 w-full">
              <div class="text-lg text-red-200 line-through mb-1">2.700.000đ</div>
              <div class="text-4xl font-black text-white">2.400.000đ</div>
              <div class="text-sm text-yellow-300 font-semibold mt-1">
                Tiết kiệm 300.000đ
              </div>
            </div>
            <ul class="text-left space-y-3 mb-8 w-full text-red-100">
              <li class="flex items-center">
                <i class="fas fa-check text-white mr-3 text-lg"></i>Tập không giới hạn
              </li>
              <li class="flex items-center">
                <i class="fas fa-check text-white mr-3 text-lg"></i>PT cá nhân 4 buổi
              </li>
              <li class="flex items-center">
                <i class="fas fa-check text-white mr-3 text-lg"></i>Chế độ dinh dưỡng chi tiết
              </li>
              <li class="flex items-center">
                <i class="fas fa-check text-white mr-3 text-lg"></i>Đánh giá thể trạng
              </li>
            </ul>
            <button class="btn btn-warning btn-block text-black font-bold text-lg py-3 rounded-full shadow-lg hover:bg-yellow-400 transform transition-transform duration-200 hover:scale-105">
              ĐĂNG KÝ NGAY
            </button>
          </div>
        </div>

        <!-- Package 3: GÓI 6 THÁNG (TIẾT KIỆM) -->
        <div class="card bg-gray-900 rounded-2xl shadow-xl border border-red-800/30 overflow-hidden transform transition-all duration-300 hover:scale-[1.02] hover:shadow-2xl hover:shadow-red-900/50">
          <div class="card-body p-8 text-center flex flex-col items-center">
            <div class="badge badge-success text-white font-bold text-base px-4 py-2 rounded-full mb-6 shadow-md">TIẾT KIỆM</div>
            <h3 class="card-title text-3xl font-extrabold text-white mb-4 justify-center">
              GÓI 6 THÁNG
            </h3>
            <div class="text-center mb-6">
              <span class="text-5xl font-black text-red-500 block">180 NGÀY</span>
            </div>
            <div class="mb-8 w-full">
              <div class="text-lg text-gray-400 line-through mb-1">5.400.000đ</div>
              <div class="text-4xl font-black text-white">4.500.000đ</div>
              <div class="text-sm text-green-400 font-semibold mt-1">
                Tiết kiệm 900.000đ
              </div>
            </div>
            <ul class="text-left space-y-3 mb-8 w-full text-gray-300">
              <li class="flex items-center">
                <i class="fas fa-check text-red-500 mr-3 text-lg"></i>Tập không giới hạn
              </li>
              <li class="flex items-center">
                <i class="fas fa-check text-red-500 mr-3 text-lg"></i>PT cá nhân 12 buổi
              </li>
              <li class="flex items-center">
                <i class="fas fa-check text-red-500 mr-3 text-lg"></i>Massage thư giãn
              </li>
              <li class="flex items-center">
                <i class="fas fa-check text-red-500 mr-3 text-lg"></i>Tặng áo gym
              </li>
            </ul>
            <button class="btn btn-error btn-block text-white font-bold text-lg py-3 rounded-full shadow-lg hover:bg-red-700 transform transition-transform duration-200 hover:scale-105">
              ĐĂNG KÝ NGAY
            </button>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
/* Custom text shadow for heading */
.custom-text-shadow {
  text-shadow: 0 0 20px rgba(239, 68, 68, 0.8), 0 0 30px rgba(239, 68, 68, 0.6);
}
</style>
