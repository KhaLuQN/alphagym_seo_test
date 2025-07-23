<template>
  <section id="packages" class="relative py-24 md:py-32 bg-gradient-to-br from-gray-950 via-red-950 to-black overflow-hidden text-white">
    <div class="relative z-10 container mx-auto px-4">
      <div class="text-center mb-16">
        <h1 class="text-5xl md:text-7xl font-extrabold mb-6 text-red-500 custom-hero-text-shadow leading-tight">
          <i class="fas fa-dumbbell mr-4 text-red-600"></i>
          <span class="text-white">Gói Tập</span>
          <span class="text-red-500"> AlphaGym</span>
        </h1>
        <p class="text-xl md:text-2xl text-gray-300 mb-10 max-w-xl mx-auto">
          Lựa chọn gói tập phù hợp với nhu cầu và mục tiêu của bạn
        </p>
      </div>

      <!-- Loading State -->
      <div v-if="isLoading" class="flex justify-center items-center py-20">
        <div class="animate-spin rounded-full h-12 w-12 border-b-2 border-red-500"></div>
        <p class="ml-4 text-lg text-gray-400">Đang tải gói tập...</p>
      </div>

      <!-- Error State -->
      <div v-else-if="apiError" class="text-center py-20">
        <div class="text-7xl mb-4 text-red-500">
          ⚠️
        </div>
        <h3 class="text-3xl font-bold text-gray-200 mb-3">
          Có lỗi xảy ra
        </h3>
        <p class="text-gray-400 text-lg">Không thể tải gói tập. Vui lòng thử lại sau.</p>
        <button @click="refreshData" class="mt-4 bg-red-500 hover:bg-red-600 text-white px-6 py-2 rounded-lg transition-colors">
          Thử lại
        </button>
      </div>

      <!-- No Data State -->
      <div v-else-if="!plans || plans.length === 0" class="text-center py-20">
        <div class="text-7xl mb-4 text-red-500">
          📦
        </div>
        <h3 class="text-3xl font-bold text-gray-200 mb-3">
          Không tìm thấy gói tập nào
        </h3>
        <p class="text-gray-400 text-lg">Hiện tại chưa có gói tập nào được cung cấp.</p>
      </div>

      <!-- Package Cards Grid -->
      <div v-else class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
        <div
          v-for="plan in sortedPlans"
          :key="`plan-${plan.plan_id}`"
          class="relative bg-gray-900 rounded-2xl shadow-xl border border-red-800/30 overflow-hidden transform transition-all duration-300 hover:scale-105 hover:shadow-2xl hover:shadow-red-900/50"
          :class="{ 'bg-gradient-to-br from-red-700 to-red-900 border-2 border-red-400': plan.is_popular }"
        >
          <!-- Popular Badge -->
          <div v-if="plan.is_popular" class="absolute -top-0 left-1/2 transform -translate-x-1/2 z-10">
            <div class="bg-yellow-400 text-black font-bold text-sm px-4 py-2 rounded-full shadow-lg">
              ⭐ PHỔ BIẾN
            </div>
          </div>

          <!-- Discount Badge -->
          <div v-else-if="plan.discount_percent > 0" class="absolute -top-0 left-1/2 transform -translate-x-1/2 z-10">
            <div class="bg-red-500 text-white font-bold text-sm px-4 py-2 rounded-full shadow-lg">
              🏷️ GIẢM {{ plan.discount_percent }}%
            </div>
          </div>

          <div class="p-8 text-center" :class="{ 'pt-12': plan.is_popular || plan.discount_percent > 0 }">
            <!-- Plan Name -->
            <h3 class="text-3xl font-extrabold text-white mb-4">
              {{ plan.plan_name }}
            </h3>

            <!-- Duration Display -->
            <div class="text-center mb-6">
              <div class="text-5xl font-black mb-2" :class="plan.is_popular ? 'text-white' : 'text-red-500'">
                {{ formatDuration(plan.duration_days) }}
              </div>
              <div class="text-sm text-gray-400">
                {{ getDurationText(plan.duration_days) }}
              </div>
            </div>

            <!-- Pricing -->
            <div class="mb-8">
              <div v-if="plan.discount_percent > 0" class="text-lg text-gray-400 line-through mb-1">
                {{ formatCurrency(plan.price) }}
              </div>
              <div class="text-4xl font-black text-white mb-2">
                {{ formatCurrency(getActualPrice(plan)) }}
              </div>
              <div v-if="plan.discount_percent > 0" class="text-sm font-semibold" :class="plan.is_popular ? 'text-yellow-300' : 'text-red-400'">
                Tiết kiệm {{ formatCurrency(getSavingAmount(plan)) }}
              </div>
              <div class="text-xs text-gray-500 mt-2">
                {{ formatCurrency(getDailyPrice(plan)) }}/ngày
              </div>
            </div>

            <!-- Features List -->
            <ul class="text-left space-y-3 mb-8" :class="plan.is_popular ? 'text-red-100' : 'text-gray-300'">
              <li class="flex items-center">
                <span class="text-lg mr-3" :class="plan.is_popular ? 'text-white' : 'text-red-500'">✓</span>
                Tập không giới hạn
              </li>
              <li class="flex items-center">
                <span class="text-lg mr-3" :class="plan.is_popular ? 'text-white' : 'text-red-500'">✓</span>
                Tư vấn chế độ dinh dưỡng
              </li>
              <li class="flex items-center">
                <span class="text-lg mr-3" :class="plan.is_popular ? 'text-white' : 'text-red-500'">✓</span>
                Sử dụng tất cả thiết bị
              </li>
              <li v-if="plan.duration_days >= 90" class="flex items-center">
                <span class="text-lg mr-3" :class="plan.is_popular ? 'text-white' : 'text-red-500'">✓</span>
                PT cá nhân {{ Math.floor(plan.duration_days / 30) * 2 }} buổi
              </li>
              <li v-if="plan.duration_days >= 180" class="flex items-center">
                <span class="text-lg mr-3" :class="plan.is_popular ? 'text-white' : 'text-red-500'">✓</span>
                Massage thư giãn miễn phí
              </li>
              <li v-if="plan.duration_days >= 365" class="flex items-center">
                <span class="text-lg mr-3" :class="plan.is_popular ? 'text-white' : 'text-red-500'">✓</span>
                Đánh giá sức khỏe định kỳ
              </li>
            </ul>

            <!-- Description -->
            <div v-if="plan.description" class="text-sm text-gray-400 mb-6 text-center">
              {{ plan.description }}
            </div>

            <!-- CTA Button -->
            <button 
              @click="selectPlan(plan)"
              class="w-full font-bold text-lg py-3 rounded-full shadow-lg transform transition-all duration-200 hover:scale-105"
              :class="plan.is_popular 
                ? 'bg-yellow-400 text-black hover:bg-yellow-500' 
                : 'bg-red-500 text-white hover:bg-red-600'"
            >
              🏋️ ĐĂNG KÝ NGAY
            </button>

            <!-- Additional Info -->
            <div class="text-xs text-gray-500 mt-3 text-center">
              🛡️ Cam kết hoàn tiền 100%
            </div>
          </div>
        </div>
      </div>

      <!-- Additional Information -->
      <div class="mt-16 text-center">
        <div class="bg-gray-900 rounded-xl p-8 border border-red-800/30">
          <h3 class="text-2xl font-bold text-red-400 mb-4">
            ℹ️ Thông tin thêm
          </h3>
          <div class="grid grid-cols-1 md:grid-cols-3 gap-6 text-gray-300">
            <div class="flex items-center justify-center">
              <div class="text-red-500 mr-3 text-xl">🕐</div>
              <div>
                <div class="font-semibold">Giờ hoạt động</div>
                <div class="text-sm">5:00 - 23:00 hàng ngày</div>
              </div>
            </div>
            <div class="flex items-center justify-center">
              <div class="text-red-500 mr-3 text-xl">📞</div>
              <div>
                <div class="font-semibold">Hotline</div>
                <div class="text-sm">1900 1234</div>
              </div>
            </div>
            <div class="flex items-center justify-center">
              <div class="text-red-500 mr-3 text-xl">📍</div>
              <div>
                <div class="font-semibold">Địa chỉ</div>
                <div class="text-sm">123 Đường ABC, TP.HCM</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'


// Mock data for testing - replace with actual API call
const mockPlans = [
  {
    plan_id: 1,
    plan_name: "Gói 1 tháng",
    duration_days: 30,
    price: 500000,
    discount_percent: 0,
    description: "Phù hợp cho người mới bắt đầu",
    is_popular: false
  },
  {
    plan_id: 2,
    plan_name: "Gói 3 tháng",
    duration_days: 90,
    price: 1350000,
    discount_percent: 10,
    description: "Gói phổ biến nhất",
    is_popular: true
  },
  {
    plan_id: 3,
    plan_name: "Gói 6 tháng",
    duration_days: 180,
    price: 2400000,
    discount_percent: 20,
    description: "Tiết kiệm tối đa",
    is_popular: false
  },
  {
    plan_id: 4,
    plan_name: "Gói 1 năm",
    duration_days: 365,
    price: 4000000,
    discount_percent: 25,
    description: "Cam kết dài hạn",
    is_popular: false
  }
]

// Reactive data
const plans = ref([])
const isLoading = ref(true)
const apiError = ref(null)

// Computed properties
const sortedPlans = computed(() => {
  return [...plans.value].sort((a, b) => {
    if (a.is_popular && !b.is_popular) return -1
    if (!a.is_popular && b.is_popular) return 1
    return a.duration_days - b.duration_days
  })
})

// Methods
const formatCurrency = (value) => {
  if (!value || isNaN(value)) return '0 ₫'
  return new Intl.NumberFormat('vi-VN', {
    style: 'currency',
    currency: 'VND',
    minimumFractionDigits: 0,
    maximumFractionDigits: 0,
  }).format(value)
}

const formatDuration = (days) => {
  if (!days || isNaN(days)) return '0 NGÀY'
  
  if (days >= 365) {
    return `${Math.floor(days / 365)} NĂM`
  } else if (days >= 30) {
    return `${Math.floor(days / 30)} THÁNG`
  } else {
    return `${days} NGÀY`
  }
}

const getDurationText = (days) => {
  if (!days || isNaN(days)) return '0 ngày'
  
  if (days >= 365) {
    const years = Math.floor(days / 365)
    const remainingDays = days % 365
    return remainingDays > 0 ? `${years} năm ${remainingDays} ngày` : `${years} năm`
  } else if (days >= 30) {
    const months = Math.floor(days / 30)
    const remainingDays = days % 30
    return remainingDays > 0 ? `${months} tháng ${remainingDays} ngày` : `${months} tháng`
  } else {
    return `${days} ngày`
  }
}

const getActualPrice = (plan) => {
  if (!plan || !plan.price) return 0
  
  if (plan.discount_percent > 0) {
    return plan.price * (1 - plan.discount_percent / 100)
  }
  return plan.price
}

const getSavingAmount = (plan) => {
  if (!plan || !plan.price || !plan.discount_percent) return 0
  return plan.price * plan.discount_percent / 100
}

const getDailyPrice = (plan) => {
  if (!plan || !plan.duration_days) return 0
  const actualPrice = getActualPrice(plan)
  return actualPrice / plan.duration_days
}

const selectPlan = (plan) => {
  console.log('Selected plan:', plan)
  alert(`Bạn đã chọn ${plan.plan_name}. Chúng tôi sẽ liên hệ với bạn sớm!`)
}

const refreshData = () => {
  loadPlans()
}

const loadPlans = async () => {
  try {
    isLoading.value = true
    apiError.value = null
    
    // Simulate API call
    await new Promise(resolve => setTimeout(resolve, 1000))
    
    // Use mock data for now - replace with actual API call
    plans.value = mockPlans
    
    console.log('Loaded plans:', plans.value)
  } catch (error) {
    console.error('Error loading plans:', error)
    apiError.value = error
  } finally {
    isLoading.value = false
  }
}

// Lifecycle
onMounted(() => {
  loadPlans()
})

// SEO
useHead({
  title: 'Gói tập gym - Bảng giá ưu đãi | AlphaGym',
  meta: [
    {
      name: 'description',
      content: 'Khám phá các gói tập gym với mức giá ưu đãi tại AlphaGym. Đa dạng gói tập phù hợp với mọi nhu cầu và ngân sách.'
    },
    {
      name: 'keywords',
      content: 'gói tập gym, bảng giá gym, membership gym, phòng tập gym, ưu đãi gym'
    }
  ]
})
</script>

<style scoped>
.custom-text-shadow {
  text-shadow: 0 0 20px rgba(239, 68, 68, 0.8), 0 0 30px rgba(239, 68, 68, 0.6);
}

.custom-hero-text-shadow {
  text-shadow: 0 0 25px rgba(239, 68, 68, 0.9), 0 0 40px rgba(239, 68, 68, 0.7);
}

/* Smooth animations */
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.grid > div {
  animation: fadeIn 0.6s ease-out;
}

/* Responsive adjustments */
@media (max-width: 768px) {
  .text-5xl {
    font-size: 2.5rem;
  }
  
  .lg\:text-6xl {
    font-size: 3rem;
  }
  
  .p-8 {
    padding: 1.5rem;
  }
}
</style>
