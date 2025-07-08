<template>
  <div>
    <h1 class="text-3xl font-bold mb-4">Tư vấn</h1>
    <form class="max-w-md mx-auto">
      <div class="mb-4">
        <label class="block mb-2">Chiều cao (cm)</label>
        <input type="number" class="input input-bordered w-full" />
      </div>
      <div class="mb-4">
        <label class="block mb-2">Cân nặng (kg)</label>
        <input type="number" class="input input-bordered w-full" />
      </div>
      <div class="mb-4">
        <label class="block mb-2">Mục tiêu</label>
        <textarea class="textarea textarea-bordered w-full"></textarea>
      </div>
      <button type="submit" class="btn btn-primary">Gửi</button>
    </form>
  </div>
</template>
