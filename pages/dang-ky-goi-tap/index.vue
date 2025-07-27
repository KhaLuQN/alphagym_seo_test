<template>
  <div class="max-w-2xl mx-auto py-12 px-4">
    <h1 class="text-3xl font-bold text-center mb-8">Đăng Ký Gói Tập</h1>

    <div class="mb-4 border-b border-gray-200">
        <ul class="flex flex-wrap -mb-px text-sm font-medium text-center" id="myTab" data-tabs-toggle="#myTabContent" role="tablist">
            <li class="mr-2" role="presentation">
                <button class="inline-block p-4 border-b-2 rounded-t-lg" id="new-customer-tab" data-tabs-target="#new-customer" type="button" role="tab" aria-controls="new-customer" aria-selected="true">Khách Hàng Mới</button>
            </li>
            <li class="mr-2" role="presentation">
                <button class="inline-block p-4 border-b-2 rounded-t-lg hover:text-gray-600 hover:border-gray-300" id="renew-tab" data-tabs-target="#renew" type="button" role="tab" aria-controls="renew" aria-selected="false">Gia Hạn Gói Tập</button>
            </li>
        </ul>
    </div>
    <div id="myTabContent">
        <div class="hidden p-4 rounded-lg bg-gray-50" id="new-customer" role="tabpanel" aria-labelledby="new-customer-tab">
            <RegisterPackageNewCustomerForm />
        </div>
        <div class="hidden p-4 rounded-lg bg-gray-50" id="renew" role="tabpanel" aria-labelledby="renew-tab">
            <RegisterPackageRenewForm />
        </div>
    </div>

  </div>
</template>

<script setup>
import { onMounted } from 'vue'
import { initFlowbite } from 'flowbite'

onMounted(() => {
    initFlowbite();
})
</script>