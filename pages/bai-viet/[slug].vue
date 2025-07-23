<template>
  <div class="min-h-screen bg-black text-white py-20 px-6 lg:px-8">
    <div class="container mx-auto">
      <NuxtLink to="/bai-viet" class="btn btn-ghost text-red-500 hover:text-red-400 mb-8 text-lg">
        <i class="fas fa-arrow-left mr-2"></i> Quay lại danh sách
      </NuxtLink>

      <!-- Loading State (Removed as we are using mock data) -->

      <!-- Error State / No Data State -->
      <div v-if="!article" class="text-center py-20">
        <div class="text-7xl mb-4 text-red-500">
          <i class="fas fa-exclamation-triangle"></i>
        </div>
        <h3 class="text-3xl font-bold text-gray-200 mb-3">
          Không tìm thấy bài viết
        </h3>
        <p class="text-gray-400 text-lg">Bài viết này có thể không tồn tại hoặc đã bị xóa.</p>
        <nuxt-link to="/bai-viet" class="inline-block mt-6 bg-red-500 hover:bg-red-600 text-white px-6 py-3 rounded-lg transition-colors">
          Về trang tin tức
        </nuxt-link>
      </div>

      <!-- Article Details -->
      <div v-else class="max-w-5xl mx-auto bg-gray-900 rounded-xl shadow-2xl border border-red-800/30 p-8 md:p-12">
        <!-- Featured Image -->
        <div class="mb-12" v-if="article?.featured_image_url">
          <img
            :src="getImageUrl(article.featured_image_url)"
            :alt="article?.title"
            class="w-full h-64 md:h-[400px] object-cover rounded-lg shadow-xl border border-red-700/50"
            @error="handleImageError"
          />
        </div>

        <!-- Article Excerpt -->
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

    <!-- Related Articles Component -->
    <RelatedArticles
      :articles="relatedArticles"
      :formatDate="formatDate"
      :getImageUrl="getImageUrl"
      :handleImageError="handleImageError"
    />

  
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";
import { useRoute } from "vue-router";
import { navigateTo, useHead } from '#app';
import RelatedArticles from '~/components/articles/RelatedArticles.vue';

const route = useRoute();
const articleSlug = route.params.slug; // Lấy slug từ URL

// Unified Mock Data (copied from index-YVoi3WOFnKtxq0o0MZJQHDnP39szlo.vue)
const unifiedMockArticles = [
  {
    article_id: 1,
    title: 'Giới thiệu trung tâm Gym mới',
    slug: 'gioi-thieu-trung-tam-gym-moi',
    content: '<p>Nội dung bài viết chi tiết về trung tâm gym...</p><p>Đây là đoạn văn thứ hai của bài viết, mô tả thêm về các tiện ích và dịch vụ.</p><ul><li>Thiết bị hiện đại</li><li>Huấn luyện viên chuyên nghiệp</li><li>Không gian rộng rãi</li></ul><p>Hãy đến và trải nghiệm ngay!</p>',
    excerpt: 'Trung tâm gym hiện đại...',
    featured_image_url: 'https://images.unsplash.com/photo-1534438327276-14873cb4eafb?w=1200&h=800&fit=crop',
    user_id: 1,
    article_category_id: 3, // Tin tức
    type: 'news',
    status: 'published',
    published_at: '2024-06-01 08:00:00',
    event_start_time: null,
    event_end_time: null,
    event_location: null,
    meta_keywords: 'gym, giới thiệu, trung tâm gym',
    meta_description: 'Giới thiệu trung tâm gym mới với trang thiết bị hiện đại và không gian tập luyện lý tưởng.',
    view_count: 150,
    created_at: '2025-06-27 12:58:32',
    updated_at: '2025-07-05 18:29:00',
    deleted_at: null,
    category: { name: 'Tin tức', slug: 'tin-tuc' },
    user: { full_name: 'Nguyễn Văn A' }
  },
  {
    article_id: 2,
    title: 'Sự kiện tập thử miễn phí',
    slug: 'su-kien-tap-thu-mien-phi',
    content: '<p>Chi tiết sự kiện tập thử...</p><p>Đăng ký ngay để nhận ưu đãi!</p>',
    excerpt: 'Tập thử miễn phí...',
    featured_image_url: 'https://images.unsplash.com/photo-1552674605-db6ffd4ed524?w=1200&h=800&fit=crop',
    user_id: 1,
    article_category_id: 2, // Sự kiện
    type: 'event',
    status: 'published',
    published_at: '2024-06-10 08:00:00',
    event_start_time: '2024-06-15 09:00:00',
    event_end_time: '2024-06-16 17:00:00',
    event_location: 'Trung tâm AlphaGym',
    meta_keywords: 'sự kiện, tập thử, miễn phí',
    meta_description: 'Tham gia tập thử miễn phí tại AlphaGym và trải nghiệm các lớp học đa dạng.',
    view_count: 200,
    created_at: '2025-06-27 12:58:32',
    updated_at: '2025-07-05 03:31:09',
    deleted_at: null,
    category: { name: 'Sự kiện', slug: 'su-kien' },
    user: { full_name: 'Trần Thị B' }
  },
  {
    article_id: 3,
    title: 'Cách giảm cân hiệu quả',
    slug: 'cach-giam-can-hieu-qua',
    content: '<p>Bài viết chia sẻ kinh nghiệm giảm cân...</p><p>Áp dụng ngay để có vóc dáng mơ ước!</p>',
    excerpt: 'Giảm cân lành mạnh...',
    featured_image_url: 'https://images.unsplash.com/photo-1512621776951-a5739dfd84f1?w=1200&h=800&fit=crop',
    user_id: 1,
    article_category_id: 4, // Blog
    type: 'blog',
    status: 'published',
    published_at: '2024-06-05 08:00:00',
    event_start_time: null,
    event_end_time: null,
    event_location: null,
    meta_keywords: 'giảm cân, sức khoẻ, dinh dưỡng',
    meta_description: 'Bí quyết giảm cân hiệu quả và an toàn với các phương pháp khoa học.',
    view_count: 320,
    created_at: '2025-06-27 12:58:32',
    updated_at: '2025-07-05 03:31:19',
    deleted_at: null,
    category: { name: 'Blog', slug: 'blog' },
    user: { full_name: 'Lê Văn C' }
  },
  {
    article_id: 4,
    title: 'Khuyến mãi tháng 7',
    slug: 'khuyen-mai-thang-7',
    content: '<p>Chi tiết chương trình khuyến mãi...</p><p>Đừng bỏ lỡ cơ hội này!</p>',
    excerpt: 'Giảm giá tới 30%...',
    featured_image_url: 'https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=1200&h=800&fit=crop',
    user_id: 1,
    article_category_id: 1, // Khuyến mãi
    type: 'promotion',
    status: 'published',
    published_at: '2025-07-07 15:48:00',
    event_start_time: null,
    event_end_time: null,
    event_location: null,
    meta_keywords: 'khuyến mãi, giảm giá, ưu đãi',
    meta_description: 'Ưu đãi đặc biệt tháng 7 với nhiều chương trình giảm giá hấp dẫn.',
    view_count: 50,
    created_at: '2025-06-27 12:58:32',
    updated_at: '2025-07-08 08:48:13',
    deleted_at: null,
    category: { name: 'Khuyến mãi', slug: 'khuyen-mai' },
    user: { full_name: 'Phạm Thị D' }
  },
  {
    article_id: 7,
    title: 'Giảm giá cho hội viên mới',
    slug: 'giam-gia-cho-hoi-vien-moi',
    content: '<p>Ưu đãi hấp dẫn cho người đăng ký mới...</p><p>Đăng ký ngay để nhận ưu đãi!</p>',
    excerpt: 'Hội viên mới nhận ưu đãi...',
    featured_image_url: 'https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=1200&h=800&fit=crop',
    user_id: 1,
    article_category_id: 1, // Khuyến mãi
    type: 'promotion',
    status: 'published',
    published_at: '2024-06-18 08:00:00',
    event_start_time: null,
    event_end_time: null,
    event_location: null,
    meta_keywords: 'khuyến mãi, hội viên mới, ưu đãi',
    meta_description: 'Ưu đãi đặc biệt khi đăng ký lần đầu tại AlphaGym.',
    view_count: 95,
    created_at: '2025-06-27 12:58:32',
    updated_at: '2025-07-08 08:32:00',
    deleted_at: null,
    category: { name: 'Khuyến mãi', slug: 'khuyen-mai' },
    user: { full_name: 'Nguyễn Văn E' }
  },
  {
    article_id: 8,
    title: 'Sự kiện thi đấu thể hình',
    slug: 'su-kien-thi-dau-the-hinh',
    content: '<p>Cuộc thi thể hình toàn thành phố...</p><p>Đăng ký tham gia ngay!</p>',
    excerpt: 'Tham gia thi đấu...',
    featured_image_url: 'https://images.unsplash.com/photo-1552674605-db6ffd4ed524?w=1200&h=800&fit=crop',
    user_id: 1,
    article_category_id: 2, // Sự kiện
    type: 'event',
    status: 'published',
    published_at: '2024-05-15 08:00:00',
    event_start_time: '2024-05-20 08:00:00',
    event_end_time: '2024-05-20 18:00:00',
    event_location: 'Sân vận động A',
    meta_keywords: 'thể hình, thi đấu, sự kiện',
    meta_description: 'Sự kiện thể hình lớn nhất năm 2024, quy tụ các vận động viên hàng đầu.',
    view_count: 500,
    created_at: '2025-06-27 12:58:32',
    updated_at: '2025-07-08 08:45:57',
    deleted_at: null,
    category: { name: 'Sự kiện', slug: 'su-kien' },
    user: { full_name: 'Trần Văn F' }
  },
  {
    article_id: 11,
    title: 'mới',
    slug: 'moi-io',
    content: '<p>dâdadddâdadddâdadddâdadddâdadddâdadddâdadddâdadd</p>',
    excerpt: 'dâdadddâdadd',
    featured_image_url: 'https://images.unsplash.com/photo-1594381837591-82977329611a?w=1200&h=800&fit=crop',
    user_id: 1,
    article_category_id: 4, // Blog
    type: 'blog',
    status: 'published',
    published_at: '2025-07-03 10:01:00',
    event_start_time: null,
    event_end_time: null,
    event_location: null,
    meta_keywords: 'mới, blog',
    meta_description: 'Bài viết mới nhất về các chủ đề sức khỏe và tập luyện.',
    view_count: 0,
    created_at: '2025-07-04 03:01:54',
    updated_at: '2025-07-08 08:46:36',
    deleted_at: null,
    category: { name: 'Blog', slug: 'blog' },
    user: { full_name: 'Lê Thị G' }
  },
  {
    article_id: 15,
    title: 'bài viết mmoiws',
    slug: 'bai-viet-mmoiws',
    content: '<p>dâdadad</p>',
    excerpt: 'dâdada',
    featured_image_url: 'https://images.unsplash.com/photo-1544367567-0f2fcb009e0b?w=1200&h=800&fit=crop',
    user_id: 1,
    article_category_id: 3, // Tin tức
    type: 'event',
    status: 'published',
    published_at: '2025-07-06 01:23:00',
    event_start_time: null,
    event_end_time: null,
    event_location: null,
    meta_keywords: 'adad, tin tức',
    meta_description: 'Bài viết mới nhất về các sự kiện sắp tới.',
    view_count: 0,
    created_at: '2025-07-05 18:23:49',
    updated_at: '2025-07-05 18:28:00',
    deleted_at: null,
    category: { name: 'Tin tức', slug: 'tin-tuc' },
    user: { full_name: 'Phạm Văn H' }
  },
  {
    article_id: 17,
    title: 'bài viết mới',
    slug: 'bai-viet-moi',
    content: '<p>dâdawdaadaddâdawdaadaddâdawdaadaddâdawdaadaddâdawdaadaddâdawdaadad</p>',
    excerpt: 'dâdawdaadad',
    featured_image_url: 'https://images.unsplash.com/photo-1594381837591-82977329611a?w=1200&h=800&fit=crop',
    user_id: 1,
    article_category_id: 2, // Sự kiện
    type: 'event',
    status: 'published',
    published_at: '2025-07-06 11:18:00',
    event_start_time: null,
    event_end_time: null,
    event_location: null,
    meta_keywords: 'dâdawdaadaddâdawdaadad, sự kiện',
    meta_description: 'Cập nhật các sự kiện mới nhất tại AlphaGym.',
    view_count: 0,
    created_at: '2025-07-06 04:19:06',
    updated_at: '2025-07-06 04:19:23',
    deleted_at: null,
    category: { name: 'Sự kiện', slug: 'su-kien' },
    user: { full_name: 'Nguyễn Thị I' }
  },
  {
    article_id: 18,
    title: 'test2',
    slug: 'test2',
    content: '<p>dâdadadadadad</p>',
    excerpt: 'dâdadadad',
    featured_image_url: 'https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=1200&h=800&fit=crop',
    user_id: 1,
    article_category_id: 3, // Tin tức
    type: 'news',
    status: 'published',
    published_at: '2025-07-07 15:48:00',
    event_start_time: null,
    event_end_time: null,
    event_location: null,
    meta_keywords: 'adad, tin tức',
    meta_description: 'Bài viết thử nghiệm 2.',
    view_count: 0,
    created_at: '2025-07-07 10:11:32',
    updated_at: '2025-07-08 08:48:27',
    deleted_at: null,
    category: { name: 'Tin tức', slug: 'tin-tuc' },
    user: { full_name: 'Trần Văn K' }
  },
  {
    article_id: 20,
    title: 'dâdada',
    slug: 'dadadad',
    content: '<p>dâda</p>',
    excerpt: 'dâda',
    featured_image_url: 'https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=1200&h=800&fit=crop',
    user_id: 1,
    article_category_id: 2, // Sự kiện
    type: 'event',
    status: 'published',
    published_at: '2025-07-09 13:31:00',
    event_start_time: null,
    event_end_time: null,
    event_location: null,
    meta_keywords: 'dâda, sự kiện',
    meta_description: 'Bài viết sự kiện mới nhất.',
    view_count: 0,
    created_at: '2025-07-09 06:31:38',
    updated_at: '2025-07-09 06:31:38',
    deleted_at: null,
    category: { name: 'Sự kiện', slug: 'su-kien' },
    user: { full_name: 'Lê Thị L' }
  }
];

const unifiedMockCategories = [
  { category_id: 1, name: 'Khuyến mãi', slug: 'khuyen-mai', articles_count: 2 },
  { category_id: 2, name: 'Sự kiện', slug: 'su-kien', articles_count: 5 },
  { category_id: 3, name: 'Tin tức', slug: 'tin-tuc', articles_count: 2 },
  { category_id: 4, name: 'Blog', slug: 'blog', articles_count: 3 },
];

// Reactive data
const comments = ref([]);
const newComment = ref({
  name: "",
  email: "",
  content: "",
});
const isSubmittingComment = ref(false);

// Simulate fetching article based on slug
const article = computed(() => {
  const foundArticle = unifiedMockArticles.find(a => a.slug === articleSlug && a.status === 'published');
  if (foundArticle) {
    // Ensure category and user are correctly linked if not already
    if (!foundArticle.category && foundArticle.article_category_id) {
      foundArticle.category = unifiedMockCategories.find(cat => cat.category_id === foundArticle.article_category_id);
    }
    // Simulate user data if not present
    if (!foundArticle.user) {
      foundArticle.user = { full_name: 'AlphaGym Team' };
    }
  }
  return foundArticle;
});

// Simulate related articles (e.g., 3 random articles excluding the current one)
const relatedArticles = computed(() => {
  return unifiedMockArticles
    .filter(a => a.slug !== articleSlug && a.status === 'published')
    .sort(() => 0.5 - Math.random()) // Random sort
    .slice(0, 3);
});

// Assign mock comments on component load
const mockComments = [
  {
    id: 1,
    name: "Minh Anh",
    content:
      "Bài viết rất hữu ích! Tôi đã áp dụng những bài tập này và thấy hiệu quả rõ rệt.",
    created_at: "2024-01-16T08:30:00Z",
  },
  {
    id: 2,
    name: "Hoàng Việt",
    content: "Cảm ơn bạn đã chia sẻ, tôi sẽ bắt đầu tập theo hướng dẫn này.",
    created_at: "2024-01-16T10:15:00Z",
  },
];

onMounted(() => {
  comments.value = mockComments;
});

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
  // Assuming API response has article.user.full_name
  if (article?.user?.full_name) {
    return article.user.full_name;
  }
  return "AlphaGym Team"; // Default author
};

const getCategoryLabel = (article) => {
  if (!article) return "Khác";

  // Try to get category name from relationship first
  if (article.category && article.category.name) {
    return article.category.name;
  }

  // Fallback to finding category by ID
  if (article.article_category_id) {
    const category = unifiedMockCategories.find(cat => cat.category_id === article.article_category_id);
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
    return '/placeholder.svg?height=800&width=1200'; // Placeholder for landscape images
  }
  // For mock data, directly return the URL as they are full URLs
  return imageUrl;
};

const handleImageError = (event) => {
  event.target.src = '/placeholder.svg?height=800&width=1200';
};

const submitComment = async () => {
  if (!newComment.value.name || !newComment.value.email || !newComment.value.content) {
    displayMessage("Vui lòng điền đầy đủ thông tin!", "error");
    return;
  }

  isSubmittingComment.value = true;

  try {
    // Simulate API call for submitting comment
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
