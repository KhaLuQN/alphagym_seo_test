<template>
  <div class="min-h-screen bg-black text-white">
    <!-- Article Header -->
    <div
      class="relative py-24 md:py-32 bg-gradient-to-br from-gray-950 via-red-950 to-black overflow-hidden"
    >
      <div class="container mx-auto px-6 relative z-10">
        <div class="max-w-4xl mx-auto text-center">
          <nav class="text-sm breadcrumbs mb-6">
            <ul class="flex justify-center text-gray-300">
              <li>
                <nuxt-link to="/" class="text-red-400 hover:text-white transition-colors duration-200">
                  <i class="fas fa-home mr-2"></i>Trang chủ
                </nuxt-link>
              </li>
              <li>
                <nuxt-link to="//bai-viet" class="text-red-400 hover:text-white transition-colors duration-200">
                  Tin tức
                </nuxt-link>
              </li>
              
              <li class="text-gray-400 font-semibold">{{ article?.title }}</li>
            </ul>
          </nav>
          
          <h1 class="text-4xl md:text-5xl font-extrabold mb-6 text-red-500 custom-text-shadow leading-tight">
            {{ article?.title }}
          </h1>

          <div
            class="flex flex-wrap justify-center items-center gap-6 text-gray-400 text-lg"
          >
            <div class="flex items-center">
              <i class="fas fa-user mr-2 text-red-400"></i>
              <span>{{ getAuthorName(article) }}</span>
            </div>
            <div class="flex items-center">
              <i class="fas fa-calendar mr-2 text-red-400"></i>
              <span>{{ formatDate(article?.published_at) }}</span>
            </div>
            <div class="flex items-center">
              <i class="fas fa-eye mr-2 text-red-400"></i>
              <span>{{ article?.view_count || 0 }} lượt xem</span>
            </div>
            <div class="flex items-center">
              <i class="fas fa-tag mr-2 text-red-400"></i>
              <span>{{ getCategoryLabel(article) }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Article Content -->
    <div class="container mx-auto px-6 py-16">
      <div v-if="isLoading" class="flex justify-center items-center py-20">
        <div class="loading loading-spinner loading-lg text-red-500"></div>
        <p class="ml-4 text-lg text-gray-400">Đang tải bài viết...</p>
      </div>
      <div v-else-if="!article || apiError" class="text-center py-20">
        <div class="text-7xl mb-4 text-red-500">
          <i class="fas fa-exclamation-triangle"></i>
        </div>
        <h3 class="text-3xl font-bold text-gray-200 mb-3">
          Không tìm thấy bài viết
        </h3>
        <p class="text-gray-400 text-lg">Bài viết này có thể không tồn tại hoặc đã bị xóa.</p>
        <p v-if="apiError" class="text-red-400 mt-4">Lỗi: {{ apiError.message }}</p>
        <nuxt-link to="/bai-viet" class="inline-block mt-6 bg-red-500 hover:bg-red-600 text-white px-6 py-3 rounded-lg transition-colors">
          Về trang tin tức
        </nuxt-link>
      </div>
      <div v-else class="max-w-4xl mx-auto bg-gray-900 rounded-xl shadow-2xl border border-red-800/30 p-8 md:p-12">
        <!-- Featured Image -->
        <div class="mb-12" v-if="article?.featured_image_url">
          <img
            :src="getImageUrl(article.featured_image_url)"
            :alt="article?.title"
            class="w-full h-64 md:h-96 object-cover rounded-lg shadow-xl border border-red-700/50"
            @error="handleImageError"
          />
        </div>

        <!-- Article Excerpt -->
        <img
  :src="article.image"
  :alt="article.title"
  class="w-full h-80 object-cover rounded-lg shadow group-hover:opacity-80 transition-opacity duration-200"
  @error="handleImageError"
/>


        <div
          class="bg-red-900/30 border-l-4 border-red-500 p-6 rounded-r-lg mb-12 shadow-md"
          v-if="article?.excerpt"
        >
          <p class="text-xl italic text-gray-300 leading-relaxed">
            {{ article?.excerpt }}
          </p>
        </div>

        <!-- Article Content -->
        <div class="prose prose-lg prose-invert max-w-none text-gray-200 leading-relaxed">
          <div
            class="text-gray-200 leading-relaxed text-lg"
            v-html="article?.content"
          ></div>
        </div>

        <!-- Meta Keywords Tags -->
        <div v-if="article?.meta_keywords" class="mt-8 pt-6 border-t border-gray-700">
          <h4 class="text-lg font-semibold mb-3 text-red-400">
            <i class="fas fa-tags mr-2"></i>Từ khóa
          </h4>
          <div class="flex flex-wrap gap-2">
            <span
              v-for="keyword in getKeywords(article.meta_keywords)"
              :key="keyword"
              class="bg-gray-800 text-gray-300 px-3 py-1 rounded-full text-sm hover:bg-red-900 cursor-pointer transition-colors"
            >
              #{{ keyword.trim() }}
            </span>
          </div>
        </div>

        <!-- Event Details (if type is 'event') -->
        <div
          class="mt-12 bg-gradient-to-r from-red-900/30 to-red-800/30 rounded-lg p-8 border border-red-600/20 shadow-lg"
          v-if="article?.type === 'event'"
        >
          <h3 class="text-2xl font-bold text-red-400 mb-6">
            <i class="fas fa-calendar-alt mr-3"></i>Thông tin sự kiện
          </h3>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div class="flex items-center" v-if="article?.event_start_time">
              <i class="fas fa-clock text-red-400 mr-3 text-xl"></i>
              <div>
                <p class="text-gray-400 text-sm">Thời gian bắt đầu</p>
                <p class="text-white font-semibold text-lg">
                  {{ formatDateTime(article?.event_start_time) }}
                </p>
              </div>
            </div>
            <div class="flex items-center" v-if="article?.event_end_time">
              <i class="fas fa-flag-checkered text-red-400 mr-3 text-xl"></i>
              <div>
                <p class="text-gray-400 text-sm">Thời gian kết thúc</p>
                <p class="text-white font-semibold text-lg">
                  {{ formatDateTime(article?.event_end_time) }}
                </p>
              </div>
            </div>
            <div class="flex items-center md:col-span-2" v-if="article?.event_location">
              <i class="fas fa-map-marker-alt text-red-400 mr-3 text-xl"></i>
              <div>
                <p class="text-gray-400 text-sm">Địa điểm</p>
                <p class="text-white font-semibold text-lg">
                  {{ article?.event_location }}
                </p>
              </div>
            </div>
          </div>
          <div class="mt-8 text-center">
            <button class="btn btn-error text-white font-bold px-8 py-3 rounded-full shadow-lg hover:bg-red-700 transform transition-transform duration-200 hover:scale-105 pulse-red">
              <i class="fas fa-ticket-alt mr-2"></i>Đăng ký tham gia
            </button>
          </div>
        </div>

        <!-- Social Share -->
        <div
          class="mt-12 bg-gray-900 rounded-xl p-6 border border-red-800/30 shadow-lg"
        >
          <h3 class="text-xl font-bold text-red-400 mb-4">
            <i class="fas fa-share-alt mr-2"></i>Chia sẻ bài viết
          </h3>
          <div class="flex flex-wrap gap-4">
            <button
              @click="shareOnFacebook"
              class="btn btn-circle btn-outline border-blue-500 text-blue-400 hover:bg-blue-500 hover:text-white transition-colors duration-200"
            >
              <i class="fab fa-facebook-f"></i>
            </button>
            <button
              @click="shareOnTwitter"
              class="btn btn-circle btn-outline border-blue-400 text-blue-400 hover:bg-blue-400 hover:text-white transition-colors duration-200"
            >
              <i class="fab fa-twitter"></i>
            </button>
            <button
              class="btn btn-circle btn-outline border-red-500 text-red-400 hover:bg-red-500 hover:text-white transition-colors duration-200"
            >
              <i class="fab fa-instagram"></i>
            </button>
            <button
              class="btn btn-circle btn-outline border-red-600 text-red-400 hover:bg-red-600 hover:text-white transition-colors duration-200"
            >
              <i class="fab fa-youtube"></i>
            </button>
            <button
              class="btn btn-circle btn-outline border-gray-500 text-gray-400 hover:bg-gray-500 hover:text-white transition-colors duration-200"
              @click="copyToClipboard"
            >
              <i class="fas fa-link"></i>
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Related Articles -->
    <div class="bg-gray-950 py-16 border-t border-red-700/50 shadow-inner shadow-red-900/20">
      <div class="container mx-auto px-6">
        <div class="max-w-6xl mx-auto">
          <h2 class="text-3xl md:text-4xl font-extrabold text-center mb-12 text-red-400 custom-text-shadow">
            <i class="fas fa-newspaper mr-3"></i>Bài viết liên quan
          </h2>

          <div v-if="relatedArticlesLoading" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
            <div v-for="n in 3" :key="n" class="bg-gray-900 rounded-xl overflow-hidden animate-pulse">
              <div class="h-48 bg-gray-700"></div>
              <div class="p-6">
                <div class="h-6 bg-gray-700 rounded mb-2"></div>
                <div class="h-4 bg-gray-700 rounded mb-4"></div>
                <div class="h-4 bg-gray-700 rounded w-3/4"></div>
              </div>
            </div>
          </div>
          <div v-else-if="relatedArticles.length === 0" class="text-center py-10">
            <p class="text-gray-400">Không có bài viết liên quan.</p>
          </div>
          <div v-else class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
            <div
              v-for="relatedArticle in relatedArticles"
              :key="relatedArticle.article_id"
              class="card bg-gray-900 rounded-xl shadow-lg border border-red-800/30 overflow-hidden transform transition-all duration-300 hover:scale-[1.02] hover:shadow-xl hover:shadow-red-900/50 cursor-pointer group"
              @click="navigateToArticle(relatedArticle.slug)"
            >
              <figure class="relative overflow-hidden h-48">
                <img
                  :src="relatedArticle.image || 'https://placehold.co/400x300/333333/EFEFEF?text=No+Image' "
                  :alt="article.title"
                  class="w-full h-48 object-cover group-hover:opacity-80 transition-opacity"
                  @error="handleImageError"
                />
              </figure>
              <div class="card-body p-6">
                <h3 class="card-title text-white text-xl font-bold mb-2 line-clamp-2 group-hover:text-red-400 transition-colors duration-300">
                  {{ relatedArticle.title }}
                </h3>
                <p class="text-gray-400 text-sm line-clamp-3 mb-4">
                  {{ relatedArticle.excerpt || 'Không có mô tả' }}
                </p>
                <div class="flex items-center justify-between mt-auto pt-4 border-t border-gray-700/50">
                  <div class="flex items-center text-gray-500 text-xs">
                    <i class="fas fa-calendar mr-1 text-red-400"></i>
                    <span>{{ formatDate(relatedArticle.published_at) }}</span>
                  </div>
                  <div class="flex items-center text-gray-500 text-xs">
                    <i class="fas fa-eye mr-1 text-red-400"></i>
                    <span>{{ relatedArticle.view_count || 0 }}</span>
                  </div>
                </div>
                <div class="card-actions justify-end mt-4">
                  <nuxt-link
                    :to="`/bai-viet/${relatedArticle.slug}`"
                    class="btn btn-error btn-sm text-white font-semibold px-4 py-2 rounded-full shadow-md transition-all duration-300 group-hover:bg-transparent group-hover:border-red-500 group-hover:text-red-400 group-hover:shadow-lg"
                  >
                    Xem thêm
                    <i class="fas fa-arrow-right ml-2"></i>
                  </nuxt-link>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Comment Section -->
    <div class="bg-gray-900 py-16 border-t border-red-700/50 shadow-inner shadow-red-900/20">
      <div class="container mx-auto px-6">
        <div class="max-w-4xl mx-auto">
          <h2 class="text-3xl md:text-4xl font-extrabold mb-8 text-red-400 custom-text-shadow">
            <i class="fas fa-comments mr-3"></i>Bình luận
          </h2>

          <!-- Comment Form -->
          <div
            class="bg-gray-900 rounded-xl p-6 border border-red-800/30 shadow-lg mb-8"
          >
            <h3 class="text-xl font-bold text-red-400 mb-4">Để lại bình luận</h3>
            <form @submit.prevent="submitComment" class="space-y-4">
              <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                <div class="form-control">
                  <label class="label">
                    <span class="label-text text-gray-300">Tên của bạn</span>
                  </label>
                  <input
                    type="text"
                    v-model="newComment.name"
                    class="input input-bordered input-md bg-gray-800 text-white placeholder-gray-400 border-red-700 focus:border-red-500 focus:ring-2 focus:ring-red-500/50 transition-all duration-300"
                    placeholder="Nhập tên của bạn"
                    required
                  />
                </div>
                <div class="form-control">
                  <label class="label">
                    <span class="label-text text-gray-300">Email</span>
                  </label>
                  <input
                    type="email"
                    v-model="newComment.email"
                    class="input input-bordered input-md bg-gray-800 text-white placeholder-gray-400 border-red-700 focus:border-red-500 focus:ring-2 focus:ring-red-500/50 transition-all duration-300"
                    placeholder="Nhập email của bạn"
                    required
                  />
                </div>
              </div>
              <div class="form-control">
                <label class="label">
                  <span class="label-text text-gray-300">Bình luận</span>
                </label>
                <textarea
                  v-model="newComment.content"
                  class="textarea textarea-bordered textarea-md bg-gray-800 text-white placeholder-gray-400 border-red-700 focus:border-red-500 focus:ring-2 focus:ring-red-500/50 h-24 transition-all duration-300"
                  placeholder="Viết bình luận của bạn..."
                  required
                ></textarea>
              </div>
              <div class="text-right">
                <button 
                  type="submit" 
                  :disabled="isSubmittingComment"
                  class="btn btn-error text-white font-bold px-8 py-3 rounded-full shadow-lg hover:bg-red-700 transform transition-transform duration-200 hover:scale-105 disabled:opacity-50"
                >
                  <i class="fas fa-paper-plane mr-2"></i>
                  {{ isSubmittingComment ? 'Đang gửi...' : 'Gửi bình luận' }}
                </button>
              </div>
            </form>
          </div>

          <!-- Comments List -->
          <div class="space-y-6">
            <div
              v-for="comment in comments"
              :key="comment.id"
              class="bg-gray-900 rounded-xl p-6 border border-red-800/30 shadow-lg"
            >
              <div class="flex items-start space-x-4">
                <div
                  class="w-12 h-12 bg-red-600 rounded-full flex items-center justify-center flex-shrink-0"
                >
                  <i class="fas fa-user text-white text-xl"></i>
                </div>
                <div class="flex-1">
                  <div class="flex items-center justify-between mb-2">
                    <h4 class="font-bold text-white text-lg">{{ comment.name }}</h4>
                    <span class="text-gray-500 text-sm">{{
                      formatDate(comment.created_at)
                    }}</span>
                  </div>
                  <p class="text-gray-300 leading-relaxed text-base">
                    {{ comment.content }}
                  </p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from "vue";
import { useRoute } from "vue-router";
import { useRuntimeConfig, useFetch, navigateTo } from '#app';


const route = useRoute();
const config = useRuntimeConfig();
const articleSlug = route.params.slug;

// Reactive data
const comments = ref([]);
const newComment = ref({
  name: "",
  email: "",
  content: "",
});
const isSubmittingComment = ref(false);

// API calls
const { data: articleResponse, pending: articlePending, error: articleError } = await useFetch(
  `${config.public.apiBase}/articles/${articleSlug}`
);

const { data: allArticlesResponse, pending: allArticlesPending, error: allArticlesError } = await useFetch(
  `${config.public.apiBase}/articles`
);

const { data: categoriesResponse, pending: categoriesPending, error: categoriesError } = await useFetch(
  `${config.public.apiBase}/article-categories`
);

// Computed properties
const article = computed(() => {
  if (articleResponse.value && articleResponse.value.data) {
    return articleResponse.value.data;
  }
  return null;
});

const relatedArticles = computed(() => {
  if (allArticlesResponse.value && Array.isArray(allArticlesResponse.value.data)) {
    return allArticlesResponse.value.data
      .filter(a => a.slug !== articleSlug)
      .slice(0, 3);
  }
  return [];
});

const categories = computed(() => {
  if (categoriesResponse.value && categoriesResponse.value.data) {
    return categoriesResponse.value.data;
  }
  return [];
});

const isLoading = computed(() => 
  articlePending.value || allArticlesPending.value || categoriesPending.value
);

const relatedArticlesLoading = computed(() => allArticlesPending.value);

const apiError = computed(() => 
  articleError.value || allArticlesError.value || categoriesError.value
);

// Methods
const formatDate = (dateString) => {
  if (!dateString) return "Chưa cập nhật";
  const date = new Date(dateString);
  return date.toLocaleDateString("vi-VN", {
    year: "numeric",
    month: "long",
    day: "numeric",
  });
};

const formatDateTime = (dateString) => {
  if (!dateString) return "Chưa cập nhật";
  const date = new Date(dateString);
  return date.toLocaleString("vi-VN", {
    year: "numeric",
    month: "long",
    day: "numeric",
    hour: "2-digit",
    minute: "2-digit",
  });
};

const getAuthorName = (article) => {
  if (article?.user?.full_name) {
    return article.user.full_name;
  }
  return "AlphaGym Team";
};

const getCategoryLabel = (article) => {
  if (!article) return "Khác";
  
  // Try to get category name from relationship first
  if (article.category && article.category.name) {
    return article.category.name;
  }
  
  // Fallback to finding category by ID
  if (article.article_category_id) {
    const category = categories.value.find(cat => cat.category_id === article.article_category_id);
    if (category) {
      return category.name;
    }
  }
  
  // Map type to Vietnamese
  const typeMap = {
    'news': 'Tin tức',
    'event': 'Sự kiện', 
    'blog': 'Blog',
    'promotion': 'Khuyến mãi'
  };
  
  return typeMap[article.type] || "Khác";
};

const getKeywords = (metaKeywords) => {
  if (!metaKeywords) return [];
  return metaKeywords.split(',').map(keyword => keyword.trim()).filter(keyword => keyword);
};

const getImageUrl = (imageUrl) => {
  if (!imageUrl) {
    return 'https://placehold.co/1200x800/333333/EFEFEF?text=No+Image';
  }
  
  // If it's already a full URL, return as is
  if (imageUrl.startsWith('http')) {
    return imageUrl;
  }
  
  // If it's a relative path, prepend the base URL
  return `${config.public.apiBase}/${imageUrl}`;
};

const handleImageError = (event) => {
  event.target.src = 'https://placehold.co/600x400/333333/EFEFEF?text=No+Image';
};

const submitComment = async () => {
  if (!newComment.value.name || !newComment.value.email || !newComment.value.content) {
    displayMessage("Vui lòng điền đầy đủ thông tin!", "error");
    return;
  }

  isSubmittingComment.value = true;

  try {
    // Simulate API call - replace with actual API call
    await new Promise(resolve => setTimeout(resolve, 1000));
    
    const comment = {
      id: Date.now(),
      name: newComment.value.name,
      content: newComment.value.content,
      created_at: new Date().toISOString(),
    };

    comments.value.unshift(comment);
    newComment.value = { name: "", email: "", content: "" };
    displayMessage("Bình luận đã được gửi thành công!", "success");
  } catch (error) {
    displayMessage("Có lỗi xảy ra khi gửi bình luận!", "error");
  } finally {
    isSubmittingComment.value = false;
  }
};

const displayMessage = (message, type) => {
  // You can implement a toast notification system here
  console.log(`Message (${type}): ${message}`);
  alert(message); // Simple alert for now
};

const copyToClipboard = async () => {
  try {
    await navigator.clipboard.writeText(window.location.href);
    displayMessage('Đã sao chép liên kết vào clipboard!', 'success');
  } catch (err) {
    // Fallback for older browsers
    const el = document.createElement('textarea');
    el.value = window.location.href;
    document.body.appendChild(el);
    el.select();
    document.execCommand('copy');
    document.body.removeChild(el);
    displayMessage('Đã sao chép liên kết vào clipboard!', 'success');
  }
};

const shareOnFacebook = () => {
  const url = encodeURIComponent(window.location.href);
  const title = encodeURIComponent(article.value?.title || '');
  window.open(`https://www.facebook.com/sharer/sharer.php?u=${url}&quote=${title}`, '_blank');
};

const shareOnTwitter = () => {
  const url = encodeURIComponent(window.location.href);
  const title = encodeURIComponent(article.value?.title || '');
  window.open(`https://twitter.com/intent/tweet?url=${url}&text=${title}`, '_blank');
};

const navigateToArticle = (slug) => {
  navigateTo(`//bai-viet/${slug}`);
};

// SEO
useHead({
  title: computed(() => article.value ? `${article.value.title} | AlphaGym` : 'Chi tiết bài viết | AlphaGym'),
  meta: [
    {
      name: "description",
      content: computed(() => article.value?.meta_description || article.value?.excerpt || "AlphaGym - Phòng tập gym hàng đầu Việt Nam"),
    },
    {
      name: "keywords",
      content: computed(() => article.value?.meta_keywords || "gym, fitness, tập luyện, sức khỏe"),
    },
    {
      property: "og:title",
      content: computed(() => article.value?.title || 'Chi tiết bài viết | AlphaGym'),
    },
    {
      property: "og:description", 
      content: computed(() => article.value?.excerpt || "AlphaGym - Phòng tập gym hàng đầu Việt Nam"),
    },
    {
      property: "og:image",
      content: computed(() => article.value ? getImageUrl(article.value.featured_image_url) : ''),
    },
  ],
});
</script>

<style scoped>
/* Custom text shadow for hero heading */
.custom-hero-text-shadow {
  text-shadow: 0 0 25px rgba(239, 68, 68, 0.9), 0 0 40px rgba(239, 68, 68, 0.7);
}

.pulse-red {
  animation: pulse-red-effect 2s infinite;
}

@keyframes pulse-red-effect {
  0% {
    box-shadow: 0 0 0 0 rgba(239, 68, 68, 0);
  }
  70% {
    box-shadow: 0 0 0 15px rgba(239, 68, 68, 0);
  }
  100% {
    box-shadow: 0 0 0 0 rgba(239, 68, 68, 0);
  }
}

/* Custom text shadow for general headings */
.custom-text-shadow {
  text-shadow: 0 0 20px rgba(239, 68, 68, 0.8), 0 0 30px rgba(239, 68, 68, 0.6);
}

.line-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.line-clamp-3 {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

/* Styling for prose content (v-html) */
.prose :where(h1):not(:where([class~="not-prose"] *)) {
  @apply text-red-400 text-4xl font-extrabold mb-4;
}
.prose :where(h2):not(:where([class~="not-prose"] *)) {
  @apply text-red-300 text-3xl font-bold mb-3;
}
.prose :where(h3):not(:where([class~="not-prose"] *)) {
  @apply text-red-200 text-2xl font-semibold mb-2;
}
.prose :where(p):not(:where([class~="not-prose"] *)) {
  @apply text-gray-300 mb-4;
}
.prose :where(ul):not(:where([class~="not-prose"] *)) {
  @apply list-disc list-inside text-gray-300 mb-4 pl-5;
}
.prose :where(li):not(:where([class~="not-prose"] *)) {
  @apply mb-2;
}
.prose :where(a):not(:where([class~="not-prose"] *)) {
  @apply text-red-400 hover:text-red-300 underline;
}
</style>