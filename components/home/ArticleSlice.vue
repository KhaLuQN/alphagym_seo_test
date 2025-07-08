<template>
  <section id="news" class="py-20 bg-black">
    <div class="container mx-auto px-4">
      <div class="text-center mb-16">
        <h2 class="text-5xl font-black mb-4 text-gradient">
          TIN TỨC & BÀI VIẾT
        </h2>
        <p class="text-xl text-gray-300">
          Cập nhật thông tin mới nhất về fitness và sức khỏe
        </p>
      </div>

      <!-- Tabs -->
      <div class="tabs tabs-boxed bg-gray-900 w-fit mx-auto mb-12">
        <button
          v-for="cat in categories"
          :key="cat"
          @click="
            () => {
              activeCategory = cat;
              visibleCount = 3;
            }
          "
          :class="[
            'tab font-semibold',
            activeCategory === cat
              ? 'tab-active text-white'
              : 'text-gray-300 hover:text-white',
          ]"
        >
          {{ cat }}
        </button>
      </div>

      <!-- Article Cards -->
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
        <article
          v-for="article in displayedArticles"
          :key="article.id"
          class="card bg-gray-900 border border-red-600/30 card-hover"
        >
          <figure class="px-6 pt-6">
            <img
              :src="article.image"
              class="w-full h-48 object-cover rounded-lg"
            />
          </figure>
          <div class="card-body">
            <div
              class="badge badge-sm mb-2"
              :class="badgeClass(article.category_name)"
            >
              {{ article.category_name.toUpperCase() }}
            </div>
            <h3 class="card-title text-lg font-bold text-white">
              {{ article.title }}
            </h3>
            <p class="text-gray-300 text-sm">{{ article.excerpt }}</p>
            <div class="flex justify-between items-center mt-4">
              <span class="text-gray-500 text-xs">{{
                timeAgo(article.publishedDate)
              }}</span>
              <NuxtLink
                :to="`/article/${article.slug}`"
                class="text-red-400 hover:text-red-300 font-semibold"
              >
                Đọc thêm
              </NuxtLink>
            </div>
          </div>
        </article>
      </div>

      <!-- Load More Button -->
      <div v-if="canLoadMore" class="text-center mt-8">
        <button
          @click="visibleCount += 3"
          class="btn btn-outline btn-sm text-white"
        >
          Xem thêm
        </button>
      </div>
    </div>
  </section>
</template>

<script setup>
const { data: response } = await useFetch("http://127.0.0.1:8000/api/articles");
const articles = computed(() => response.value?.data || []);

const categories = computed(() => {
  const list = new Set(articles.value.map((a) => a.category_name));
  return ["Tất cả", ...list];
});

const activeCategory = ref("Tất cả");
const visibleCount = ref(3);

const filteredArticles = computed(() => {
  if (activeCategory.value === "Tất cả") return articles.value;
  return articles.value.filter((a) => a.category_name === activeCategory.value);
});

// Hiển thị slice
const displayedArticles = computed(() =>
  filteredArticles.value.slice(0, visibleCount.value)
);

// Kiểm tra còn bài để load không
const canLoadMore = computed(
  () => visibleCount.value < filteredArticles.value.length
);

// Badge class theo category
const badgeClass = (category) => {
  if (category.includes("Fitness")) return "badge-error";
  if (category.includes("Dinh dưỡng")) return "badge-success";
  if (category.includes("Kiến thức") || category.includes("Tim mạch"))
    return "badge-info";
  return "badge-neutral";
};

// Thời gian định dạng
const timeAgo = (isoDate) => {
  const date = new Date(isoDate);
  const now = new Date();
  const diff = (now - date) / 1000;

  if (diff < 60) return "Vừa xong";
  if (diff < 3600) return `${Math.floor(diff / 60)} phút trước`;
  if (diff < 86400) return `${Math.floor(diff / 3600)} giờ trước`;
  if (diff < 604800) return `${Math.floor(diff / 86400)} ngày trước`;
  return date.toLocaleDateString("vi-VN");
};
</script>
