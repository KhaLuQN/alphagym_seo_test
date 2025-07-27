//components/RegisterPackage/RenewForm.vue
<template>
  <form @submit.prevent="submitForm" class="space-y-6">
    <!-- Member Information Section -->
    <div class="card bg-gray-800 border border-red-900/30">
      <div class="card-body">
        <h3 class="text-lg font-bold text-white mb-4 flex items-center">
          <i class="fas fa-id-card text-red-500 mr-2"></i>
          Thông Tin Thành Viên
        </h3>

        <div class="form-control">
          <label class="label">
            <span class="label-text text-gray-300 font-medium">
              <i class="fas fa-phone mr-2 text-red-400"></i>
              Số điện thoại đã đăng ký
            </span>
          </label>
          <div class="relative">
            <input
              type="tel"
              id="phone"
              v-model="formData.member_identifier"
              required
              class="input input-bordered bg-gray-700 border-gray-600 text-white focus:border-red-500 focus:ring-red-500 pl-12"
              placeholder="Nhập số điện thoại của bạn"
            />
            <i
              class="fas fa-mobile-alt absolute left-4 top-1/2 transform -translate-y-1/2 text-red-400"
            ></i>
          </div>
          <label class="label">
            <span class="label-text-alt text-gray-400">
              <i class="fas fa-info-circle mr-1"></i>
              Nhập số điện thoại bạn đã sử dụng khi đăng ký lần đầu
            </span>
          </label>
        </div>
      </div>
    </div>

    <!-- Package Selection Section -->
    <div class="card bg-gray-800 border border-red-900/30">
      <div class="card-body">
        <h3 class="text-lg font-bold text-white mb-4 flex items-center">
          <i class="fas fa-dumbbell text-red-500 mr-2"></i>
          Chọn Gói Gia Hạn
        </h3>

        <div class="grid gap-4">
          <div v-for="pkg in packages" :key="pkg.id" class="package-option">
            <label
              class="cursor-pointer"
              :class="{ selected: formData.membership_plan_id == pkg.id }"
            >
              <input
                type="radio"
                :value="pkg.id"
                v-model="formData.membership_plan_id"
                class="sr-only"
                required
              />
              <div
                class="card bg-gray-700 border-2 transition-all duration-300 hover:border-red-500 hover:shadow-lg"
                :class="{
                  'border-red-500 bg-red-900/20 shadow-lg':
                    formData.membership_plan_id == pkg.id,
                  'border-gray-600': formData.membership_plan_id != pkg.id,
                }"
              >
                <div class="card-body p-4">
                  <div class="flex items-center justify-between">
                    <div class="flex-1">
                      <div class="flex items-center mb-2">
                        <h4 class="text-white font-bold text-lg">
                          {{ pkg.name }}
                        </h4>
                        <div
                          v-if="pkg.discount_percent > 0"
                          class="badge badge-error ml-2 animate-pulse"
                        >
                          -{{ pkg.discount_percent }}%
                        </div>
                        <div class="badge badge-success ml-2">
                          <i class="fas fa-redo-alt mr-1"></i>
                          Gia hạn
                        </div>
                      </div>

                      <div class="flex items-center mb-2">
                        <span class="text-2xl font-bold text-red-500">
                          {{ formatPrice(pkg.price) }}đ
                        </span>
                        <span class="text-gray-400 ml-2"
                          >/ {{ pkg.duration_days }} ngày</span
                        >
                      </div>

                      <p class="text-gray-300 text-sm mb-3">
                        {{ pkg.description }}
                      </p>

                      <!-- Features -->
                      <div
                        v-if="pkg.features && pkg.features.length > 0"
                        class="space-y-1"
                      >
                        <div
                          v-for="feature in pkg.features"
                          :key="feature.name"
                          class="flex items-center text-sm text-gray-300"
                        >
                          <i class="fas fa-check text-green-400 mr-2"></i>
                          {{ feature.name }}
                        </div>
                      </div>
                    </div>

                    <div class="ml-4">
                      <div
                        class="w-6 h-6 rounded-full border-2 flex items-center justify-center"
                        :class="{
                          'border-red-500 bg-red-500':
                            formData.membership_plan_id == pkg.id,
                          'border-gray-400':
                            formData.membership_plan_id != pkg.id,
                        }"
                      >
                        <i
                          v-if="formData.membership_plan_id == pkg.id"
                          class="fas fa-check text-white text-xs"
                        ></i>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </label>
          </div>
        </div>
      </div>
    </div>

    <!-- Renewal Benefits -->
    <div
      class="card bg-gradient-to-r from-red-900/20 to-gray-800 border border-red-900/30"
    >
      <div class="card-body">
        <h3 class="text-lg font-bold text-white mb-4 flex items-center">
          <i class="fas fa-gift text-red-500 mr-2"></i>
          Ưu Đãi Gia Hạn
        </h3>

        <div class="grid md:grid-cols-3 gap-4">
          <div class="flex items-center text-gray-300">
            <i class="fas fa-clock text-red-400 mr-3"></i>
            <span class="text-sm">Không gián đoạn thời gian tập</span>
          </div>
          <div class="flex items-center text-gray-300">
            <i class="fas fa-star text-red-400 mr-3"></i>
            <span class="text-sm">Ưu tiên đặt lịch tập</span>
          </div>
          <div class="flex items-center text-gray-300">
            <i class="fas fa-medal text-red-400 mr-3"></i>
            <span class="text-sm">Tích lũy điểm thưởng</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Submit Button -->
    <div class="text-center pt-4">
      <button
        type="submit"
        :disabled="isSubmitting"
        class="btn btn-lg bg-gradient-to-r from-red-600 to-red-700 hover:from-red-700 hover:to-red-800 border-none text-white font-bold px-12 py-4 text-lg shadow-lg hover:shadow-xl transition-all duration-300 transform hover:scale-105"
      >
        <span v-if="!isSubmitting" class="flex items-center">
          <i class="fas fa-redo-alt mr-2"></i>
          GIA HẠN NGAY
        </span>
        <span v-else class="flex items-center">
          <i class="fas fa-spinner fa-spin mr-2"></i>
          Đang xử lý...
        </span>
      </button>

      <p class="text-gray-400 text-sm mt-4">
        <i class="fas fa-shield-alt mr-1"></i>
        Gia hạn tự động từ ngày hết hạn gói cũ
      </p>
    </div>
  </form>
</template>

<script setup>
import { ref } from "vue";
import { useApiFetch } from "@/composables/useApiFetch";

const formData = ref({
  member_identifier: "",
  membership_plan_id: "",
});

const packages = ref([]);
const isSubmitting = ref(false);

const { data, error } = await useApiFetch("membership-plans");
if (data.value) {
  packages.value = data.value.data;
}

const formatPrice = (price) => {
  return new Intl.NumberFormat("vi-VN").format(price);
};

const submitForm = async () => {
  isSubmitting.value = true;

  const payload = {
    customer_type: "existing",
    member_identifier: formData.value.member_identifier,
    membership_plan_id: formData.value.membership_plan_id,
    payment_method: "vnpay",
  };

  console.log("Renew Form Data:", payload);

  try {
    const { data: responseData, error: responseError } = await useApiFetch(
      "/subscription/initiate",
      {
        method: "POST",
        body: payload,
      }
    );

    if (responseData.value && responseData.value.payment_url) {
      window.location.href = responseData.value.payment_url;
    } else if (responseData.value) {
      alert("Gia hạn thành công nhưng không nhận được URL thanh toán.");
    } else if (responseError.value) {
      let errorMessage = "Lỗi khi gia hạn: ";
      if (
        responseError.value.statusCode === 422 &&
        responseError.value.data &&
        responseError.value.data.errors
      ) {
        for (const key in responseError.value.data.errors) {
          errorMessage += `\n- ${responseError.value.data.errors[key].join(
            ", "
          )}`;
        }
      } else {
        errorMessage += responseError.value.message;
      }
      alert(errorMessage);
      console.error("Renewal error:", responseError.value);
    }
  } catch (e) {
    alert("Đã xảy ra lỗi không mong muốn.");
    console.error("Unexpected error:", e);
  } finally {
    isSubmitting.value = false;
  }
};
</script>

<style scoped>
.package-option .card {
  transition: all 0.3s ease;
}

.package-option:hover .card {
  transform: translateY(-2px);
}

.package-option.selected .card {
  box-shadow: 0 10px 25px rgba(239, 68, 68, 0.3);
}
</style>
