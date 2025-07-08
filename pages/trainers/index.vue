<template>
  <div>
    <h1 class="text-3xl font-bold mb-4">Huấn luyện viên</h1>
    <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
      <div v-for="trainer in 8" :key="trainer" class="border p-4 rounded-lg text-center">
        <NuxtLink :to="`/trainers/${trainer}`">
          <div class="w-32 h-32 bg-gray-200 rounded-full mx-auto mb-4"></div>
          <h2 class="text-xl font-bold">Tên HLV {{ trainer }}</h2>
        </NuxtLink>
      </div>
    </div>
  </div>
</template>
