<template>
  <!-- Related Articles -->
  <div class="bg-gray-900/30 py-16">
    <div class="container mx-auto px-4">
      <div class="max-w-6xl mx-auto">
        <h2 class="text-3xl font-bold text-center mb-12 text-gradient">
          <i class="fas fa-newspaper mr-3"></i>Bài viết liên quan
        </h2>

        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
          <div
            v-for="relatedArticle in relatedArticles"
            :key="relatedArticle.id"
            class="card bg-gray-800/50 border border-red-600/20 card-hover cursor-pointer"
          >
            <figure class="px-6 pt-6">
              <img
                :src="
                  relatedArticle.featured_image_url || '/images/placeholder.jpg'
                "
                :alt="relatedArticle.title"
                class="rounded-lg h-48 w-full object-cover"
              />
            </figure>
            <div class="card-body">
              <h3 class="card-title text-white text-lg line-clamp-2">
                {{ relatedArticle.title }}
              </h3>
              <p class="text-gray-400 text-sm line-clamp-3">
                {{ relatedArticle.excerpt }}
              </p>
              <div class="flex items-center justify-between mt-4">
                <div class="flex items-center text-gray-500 text-sm">
                  <i class="fas fa-calendar mr-1"></i>
                  <span>{{ formatDate(relatedArticle.published_at) }}</span>
                </div>
                <div class="flex items-center text-gray-500 text-sm">
                  <i class="fas fa-eye mr-1"></i>
                  <span>{{ relatedArticle.view_count || 0 }}</span>
                </div>
              </div>
              <div class="card-actions justify-end mt-4">
                <nuxt-link
                  :to="`/articles/${relatedArticle.id}`"
                  class="btn btn-error btn-sm text-white"
                >
                  Xem thêm
                </nuxt-link>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
const props = defineProps({
  relatedArticles: {
    type: Array,
    required: true,
  },
});
const formatDate = (dateString) => {
  if (!dateString) return "Chưa cập nhật";

  const date = new Date(dateString);
  return date.toLocaleDateString("vi-VN", {
    year: "numeric",
    month: "long",
    day: "numeric",
  });
};
</script>
