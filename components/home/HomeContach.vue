<template>
  <section id="contact" class="py-20 bg-gradient-to-r from-red-600 to-red-800">
    <div class="container mx-auto px-4">
      <div class="text-center mb-16">
        <h2 class="text-5xl font-black mb-4 text-white">LIÊN HỆ</h2>
        <p class="text-xl text-red-100">Sẵn sàng bắt đầu hành trình của bạn?</p>
      </div>
      <div class="grid grid-cols-1 md:grid-cols-2 gap-12">
        <div class="space-y-6">
          <div class="flex items-center space-x-4">
            <div
              class="w-12 h-12 bg-white rounded-full flex items-center justify-center"
            >
              <i class="fas fa-map-marker-alt text-red-600"></i>
            </div>
            <div>
              <h3 class="font-bold text-white">Địa chỉ</h3>
              <p class="text-red-100">123 Đường Fitness, Quận 1, TP.HCM</p>
            </div>
          </div>
          <div class="flex items-center space-x-4">
            <div
              class="w-12 h-12 bg-white rounded-full flex items-center justify-center"
            >
              <i class="fas fa-phone text-red-600"></i>
            </div>
            <div>
              <h3 class="font-bold text-white">Điện thoại</h3>
              <p class="text-red-100">0123 456 789</p>
            </div>
          </div>
          <div class="flex items-center space-x-4">
            <div
              class="w-12 h-12 bg-white rounded-full flex items-center justify-center"
            >
              <i class="fas fa-envelope text-red-600"></i>
            </div>
            <div>
              <h3 class="font-bold text-white">Email</h3>
              <p class="text-red-100">info@powergym.vn</p>
            </div>
          </div>
        </div>
        <div class="card bg-white/10 backdrop-blur-sm border border-white/20">
          <div class="card-body">
            <form class="space-y-4">
              <div class="form-control">
                <input
                  type="text"
                  placeholder="Họ tên"
                  class="input input-bordered bg-white/20 text-white placeholder-white/70"
                />
              </div>
              <div class="form-control">
                <input
                  type="email"
                  placeholder="Email"
                  class="input input-bordered bg-white/20 text-white placeholder-white/70"
                />
              </div>
              <div class="form-control">
                <input
                  type="tel"
                  placeholder="Số điện thoại"
                  class="input input-bordered bg-white/20 text-white placeholder-white/70"
                />
              </div>
              <div class="form-control">
                <textarea
                  placeholder="Tin nhắn"
                  class="textarea textarea-bordered bg-white/20 text-white placeholder-white/70 h-24"
                ></textarea>
              </div>
              <button class="btn btn-warning btn-block font-bold text-black">
                <i class="fas fa-paper-plane mr-2"></i>GỬI TIN NHẮN
              </button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
