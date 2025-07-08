<template>
  <div>
    <h1 class="text-3xl font-bold mb-4">Bài viết</h1>
    <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
      <div v-for="post in 6" :key="post" class="border p-4 rounded-lg">
        <NuxtLink :to="`/posts/${post}`">
          <div class="h-48 bg-gray-200 rounded-lg mb-4"></div>
          <h2 class="text-xl font-bold">Tiêu đề bài viết {{ post }}</h2>
        </NuxtLink>
      </div>
    </div>
  </div>
</template>
