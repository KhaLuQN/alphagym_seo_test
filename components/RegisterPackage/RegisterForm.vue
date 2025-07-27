<template>
  <div class="max-w-2xl mx-auto py-12 px-4">
    <h1 class="text-3xl font-bold text-center mb-8">Đăng Ký Gói Tập</h1>
    <form @submit.prevent="submitForm" class="space-y-6">
      <div>
        <label for="name" class="block text-sm font-medium text-gray-700">Họ và tên</label>
        <input type="text" id="name" v-model="formData.name" required class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
      </div>
      <div>
        <label for="phone" class="block text-sm font-medium text-gray-700">Số điện thoại</label>
        <input type="tel" id="phone" v-model="formData.phone" required class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
      </div>
      <div>
        <label for="email" class="block text-sm font-medium text-gray-700">Email</label>
        <input type="email" id="email" v-model="formData.email" required class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
      </div>
      <div>
        <label for="package" class="block text-sm font-medium text-gray-700">Chọn gói tập</label>
        <select id="package" v-model="formData.package" required class="mt-1 block w-full px-3 py-2 border border-gray-300 bg-white rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
          <option disabled value="">Vui lòng chọn một gói</option>
          <option v-for="pkg in packages" :key="pkg.id" :value="pkg.id">{{ pkg.name }} - {{ pkg.price }}đ</option>
        </select>
      </div>
       <div>
        <label for="message" class="block text-sm font-medium text-gray-700">Lời nhắn (tùy chọn)</label>
        <textarea id="message" v-model="formData.message" rows="4" class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"></textarea>
      </div>
      <div>
        <button type="submit" class="w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
          Đăng Ký
        </button>
      </div>
    </form>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useApiFetch } from '@/composables/useApiFetch'

const formData = ref({
  name: '',
  phone: '',
  email: '',
  package: '',
  message: ''
})

const packages = ref([])

const { data, error } = await useApiFetch('membership-plans')
if (data.value) {
  packages.value = data.value.data
}

const submitForm = () => {
  console.log('Form Data:', formData.value)
  alert('Kiểm tra console để xem dữ liệu đã gửi.')
}
</script>
