<template>
  <section id="packages" class="py-20 bg-gray-950 text-white">
    <div class="container mx-auto px-6 lg:px-8">
      <div class="text-center mb-16">
        <h2
          class="text-5xl lg:text-6xl font-extrabold mb-4 text-red-500 custom-text-shadow"
        >
          GÓI TẬP ALPHAGYM
        </h2>
        <p class="text-xl text-gray-300 max-w-2xl mx-auto">
          Lựa chọn gói tập phù hợp với nhu cầu và mục tiêu của bạn
        </p>
      </div>

      <!-- Wrapper cho Swiper Slider -->
      <div class="relative">
        <swiper
          :modules="[Navigation, Autoplay]"
          :slides-per-view="1"
          :space-between="40"
          :loop="true"
          :centered-slides="true"
          :autoplay="{
            delay: 5000,
            disableOnInteraction: false,
          }"
          :navigation="{
            nextEl: '.plan-swiper-button-next',
            prevEl: '.plan-swiper-button-prev',
          }"
          :breakpoints="{
            '768': {
              slidesPerView: 2,
              centeredSlides: false,
            },
            '1024': {
              slidesPerView: 3,
              centeredSlides: false,
            },
          }"
          class="pb-16"
        >
          <!-- Lặp qua danh sách Gói tập được truyền vào -->
          <swiper-slide v-for="plan in plans" :key="plan.id" class="h-full">
            <div
              class="card rounded-2xl shadow-xl border overflow-hidden transform transition-all duration-300 h-full flex flex-col"
              :class="
                isPopularPlan(plan)
                  ? 'bg-gradient-to-br from-red-700 to-red-900 border-2 border-red-400 shadow-red-500/30'
                  : 'bg-gray-900 border-red-800/30 hover:scale-[1.02] hover:shadow-2xl hover:shadow-red-900/50'
              "
            >
              <div
                class="card-body p-8 text-center flex flex-col items-center flex-grow"
              >
                <!-- Badge cho gói miễn phí hoặc khuyến mãi -->
                <div
                  v-if="getBadgeText(plan)"
                  class="badge text-white font-bold text-base px-4 py-3 rounded-full mb-6 shadow-md"
                  :class="
                    isPopularPlan(plan)
                      ? 'badge-warning text-black'
                      : 'badge-error'
                  "
                >
                  {{ getBadgeText(plan) }}
                </div>

                <h3
                  class="card-title text-3xl font-extrabold text-white mb-4 justify-center"
                >
                  {{ plan.name }}
                </h3>

                <!-- Hiển thị thời hạn -->
                <div class="text-center mb-6">
                  <span
                    class="text-5xl font-black block"
                    :class="isPopularPlan(plan) ? 'text-white' : 'text-red-500'"
                  >
                    {{ plan.duration_days }}
                  </span>
                </div>

                <!-- Hiển thị giá -->
                <div class="mb-8 w-full">
                  <div
                    v-if="parseFloat(plan.discount_percent) > 0"
                    class="text-lg line-through mb-1"
                    :class="
                      isPopularPlan(plan) ? 'text-red-200' : 'text-gray-400'
                    "
                  >
                    {{ formatCurrency(parseFloat(plan.price)) }}
                  </div>
                  <div class="text-4xl font-black text-white">
                    {{ formatCurrency(getActualPrice(plan)) }}
                  </div>
                  <div
                    v-if="parseFloat(plan.discount_percent) > 0"
                    class="text-sm font-semibold mt-1"
                    :class="
                      isPopularPlan(plan) ? 'text-yellow-300' : 'text-red-400'
                    "
                  >
                    Tiết kiệm {{ parseFloat(plan.discount_percent) }}%
                  </div>
                </div>

                <!-- Danh sách tính năng -->
                <ul
                  class="text-left space-y-3 mb-8 w-full flex-grow"
                  :class="
                    isPopularPlan(plan) ? 'text-red-100' : 'text-gray-300'
                  "
                >
                  <li
                    v-for="(feature, index) in plan.features"
                    :key="index"
                    class="flex items-center"
                  >
                    <span
                      class="text-lg mr-3"
                      :class="
                        isPopularPlan(plan) ? 'text-white' : 'text-red-500'
                      "
                    >
                      <i class="fas fa-check text-green-500 mr-3 text-lg"></i>
                    </span>
                    <span class="flex-1">
                      {{ feature.name }}
                      <strong v-if="feature.value" class="text-white ml-2">
                        ({{ feature.value }})
                      </strong>
                    </span>
                  </li>
                </ul>
                <!-- Mô tả gói -->
                <p
                  class="text-sm mb-6 opacity-90"
                  :class="
                    isPopularPlan(plan) ? 'text-red-100' : 'text-gray-300'
                  "
                >
                  {{ plan.description }}
                </p>
                <NuxtLink
                  to="/dang-ky-goi-tap"
                  class="btn btn-block font-bold text-lg py-3 rounded-full shadow-lg transform transition-transform duration-200 hover:scale-105 mt-auto"
                  :class="
                    isPopularPlan(plan)
                      ? 'btn-warning text-black'
                      : 'btn-error text-white'
                  "
                >
                  ĐĂNG KÝ
                </NuxtLink>
              </div>
            </div>
          </swiper-slide>
        </swiper>

        <!-- Nút điều hướng tùy chỉnh -->
        <div class="plan-swiper-button-prev swiper-button-custom">
          <i class="fas fa-chevron-left"></i>
        </div>
        <div class="plan-swiper-button-next swiper-button-custom">
          <i class="fas fa-chevron-right"></i>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
// Import các thành phần cần thiết từ Swiper
import { Swiper, SwiperSlide } from "swiper/vue";
import { Navigation, Autoplay } from "swiper/modules";
import { ref, computed } from "vue";

// Import CSS của Swiper
import "swiper/css";
import "swiper/css/navigation";

const props = defineProps({
  plans: {
    type: Array,
    required: true,
  },
});

// --- CÁC HÀM HELPER ---
const formatCurrency = (value) => {
  if (value === 0) return "Miễn phí";
  return new Intl.NumberFormat("vi-VN", {
    style: "currency",
    currency: "VND",
  }).format(value);
};

const getActualPrice = (plan) => {
  const price = parseFloat(plan.price);
  const discount = parseFloat(plan.discount_percent);
  return price * (1 - discount / 100);
};

const getBadgeText = (plan) => {
  const price = parseFloat(plan.price);
  const discount = parseFloat(plan.discount_percent);

  if (price === 0 || discount === 100) return "MIỄN PHÍ";
  if (discount > 0) return `GIẢM ${discount}%`;
  if (isPopularPlan(plan)) return "PHỔ BIẾN";
  return null;
};

const isPopularPlan = (plan) => {
  return plan.name.trim().toLowerCase().endsWith("plus");
};
</script>

<style scoped>
/* Custom text shadow for heading */
.custom-text-shadow {
  text-shadow: 0 0 20px rgba(239, 68, 68, 0.8), 0 0 30px rgba(239, 68, 68, 0.6);
}

/* Style cho các nút điều hướng của Swiper */
.swiper-button-custom {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 50px;
  height: 50px;
  background-color: rgba(0, 0, 0, 0.3);
  border-radius: 50%;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
  z-index: 10;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.swiper-button-custom:hover {
  background-color: #dc2626;
}

.plan-swiper-button-prev {
  left: 0px;
}

.plan-swiper-button-next {
  right: 0px;
}

/* Ẩn nút trên mobile */
@media (max-width: 768px) {
  .swiper-button-custom {
    display: none;
  }
}
</style>
