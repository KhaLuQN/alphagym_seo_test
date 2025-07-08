<template>
  <div>
    <h1 class="text-3xl font-bold mb-4">Đánh giá của khách hàng</h1>
    <div class="space-y-4">
      <div v-for="review in 5" :key="review" class="border p-4 rounded-lg">
        <p>"Đây là một bài đánh giá tuyệt vời."</p>
        <p class="text-right font-bold">- Tên khách hàng {{ review }}</p>
      </div>
    </div>
  </div>
</template>
