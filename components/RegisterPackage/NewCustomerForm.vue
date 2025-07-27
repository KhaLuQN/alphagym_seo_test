<template>
  <form @submit.prevent="submitForm" class="space-y-6">
    <div>
      <label for="name" class="block text-sm font-medium text-gray-700"
        >Họ và tên</label
      >
      <input
        type="text"
        id="name"
        v-model="formData.customer_name"
        required
        class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
      />
    </div>
    <div>
      <label for="phone" class="block text-sm font-medium text-gray-700"
        >Số điện thoại</label
      >
      <input
        type="tel"
        id="phone"
        v-model="formData.customer_phone"
        required
        class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
      />
    </div>
    <div>
      <label for="email" class="block text-sm font-medium text-gray-700"
        >Email</label
      >
      <input
        type="email"
        id="email"
        v-model="formData.customer_email"
        required
        class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
      />
    </div>
    <div>
      <label for="package" class="block text-sm font-medium text-gray-700"
        >Chọn gói tập</label
      >
      <select
        id="package"
        v-model="formData.membership_plan_id"
        required
        class="mt-1 block w-full px-3 py-2 border border-gray-300 bg-white rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
      >
        <option disabled value="">Vui lòng chọn một gói</option>
        <option v-for="pkg in packages" :key="pkg.id" :value="pkg.id">
          {{ pkg.name }} - {{ pkg.price }}đ
        </option>
      </select>
    </div>
    <div>
      <label for="message" class="block text-sm font-medium text-gray-700"
        >Lời nhắn (tùy chọn)</label
      >
      <textarea
        id="message"
        v-model="formData.message"
        rows="4"
        class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
      ></textarea>
    </div>
    <div>
      <button
        type="submit"
        class="w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
      >
        Đăng Ký
      </button>
    </div>
  </form>
</template>

<script setup>
import { ref } from "vue";
import { useApiFetch } from "@/composables/useApiFetch";

const formData = ref({
  customer_name: "",
  customer_phone: "",
  customer_email: "",
  membership_plan_id: "",
  message: "",
});

const packages = ref([]);

const { data, error } = await useApiFetch("membership-plans");
if (data.value) {
  packages.value = data.value.data;
}

const submitForm = async () => {
  const payload = {
    customer_type: "new",
    customer_name: formData.value.customer_name,
    customer_phone: formData.value.customer_phone,
    customer_email: formData.value.customer_email,
    membership_plan_id: formData.value.membership_plan_id,
    payment_method: "vnpay",
  };

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
      alert("Đăng ký thành công nhưng không nhận được URL thanh toán.");
    } else if (responseError.value) {
      let errorMessage = "Lỗi khi đăng ký: ";
      if (responseError.value.statusCode === 422 && responseError.value.data && responseError.value.data.errors) {
        for (const key in responseError.value.data.errors) {
          errorMessage += `\n- ${responseError.value.data.errors[key].join(', ')}`;
        }
      } else {
        errorMessage += responseError.value.message;
      }
      alert(errorMessage);
      console.error("Registration error:", responseError.value);
    }
  } catch (e) {
    alert("Đã xảy ra lỗi không mong muốn.");
    console.error("Unexpected error:", e);
  }
};
</script>
