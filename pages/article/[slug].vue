<template>
  <div class="min-h-screen bg-black text-white">
    <!-- Article Header -->
    <div
      class="relative py-20 bg-gradient-to-r from-red-900 via-red-800 to-black"
    >
      <div class="container mx-auto px-4">
        <div class="max-w-4xl mx-auto text-center">
          <nav class="text-sm breadcrumbs mb-6">
            <ul class="flex justify-center">
              <li>
                <nuxt-link to="/" class="text-red-400 hover:text-white">
                  <i class="fas fa-home mr-2"></i>Trang chủ
                </nuxt-link>
              </li>
              <li>
                <nuxt-link to="/articles" class="text-red-400 hover:text-white">
                  Tin tức
                </nuxt-link>
              </li>
              <li class="text-gray-400">{{ article.title }}</li>
            </ul>
          </nav>

          <h1 class="text-4xl md:text-5xl font-bold mb-6 text-gradient">
            {{ article.title }}
          </h1>

          <div
            class="flex flex-wrap justify-center items-center gap-6 text-gray-300"
          >
            <div class="flex items-center">
              <i class="fas fa-user mr-2 text-red-400"></i>
              <span>{{ article.author || "PowerGym Team" }}</span>
            </div>
            <div class="flex items-center">
              <i class="fas fa-calendar mr-2 text-red-400"></i>
              <span>{{ formatDate(article.published_at) }}</span>
            </div>
            <div class="flex items-center">
              <i class="fas fa-eye mr-2 text-red-400"></i>
              <span>{{ article.view_count || 0 }} lượt xem</span>
            </div>
            <div class="flex items-center">
              <i class="fas fa-tag mr-2 text-red-400"></i>
              <span>{{ article.category || "Tin tức" }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Article Content -->
    <div class="container mx-auto px-4 py-16">
      <div class="max-w-4xl mx-auto">
        <!-- Featured Image -->
        <div class="mb-12" v-if="article.featured_image_url">
          <img
            :src="article.featured_image_url"
            :alt="article.title"
            class="w-full h-64 md:h-96 object-cover rounded-lg shadow-2xl"
          />
        </div>

        <!-- Article Excerpt -->
        <div
          class="bg-red-900/20 border-l-4 border-red-500 p-6 rounded-r-lg mb-12"
          v-if="article.excerpt"
        >
          <p class="text-xl italic text-gray-300 leading-relaxed">
            {{ article.excerpt }}
          </p>
        </div>

        <!-- Article Content -->
        <div class="prose prose-lg prose-invert max-w-none">
          <div
            class="text-gray-300 leading-relaxed text-lg"
            v-html="article.content"
          ></div>
        </div>

        <!-- Event Details (if type is 'event') -->
        <div
          class="mt-12 bg-gradient-to-r from-red-900/30 to-red-800/30 rounded-lg p-8 border border-red-600/20"
          v-if="article.type === 'event'"
        >
          <h3 class="text-2xl font-bold text-red-400 mb-6">
            <i class="fas fa-calendar-alt mr-3"></i>Thông tin sự kiện
          </h3>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div class="flex items-center">
              <i class="fas fa-clock text-red-400 mr-3"></i>
              <div>
                <p class="text-gray-400 text-sm">Thời gian bắt đầu</p>
                <p class="text-white font-semibold">
                  {{ formatDateTime(article.event_start_time) }}
                </p>
              </div>
            </div>
            <div class="flex items-center">
              <i class="fas fa-flag-checkered text-red-400 mr-3"></i>
              <div>
                <p class="text-gray-400 text-sm">Thời gian kết thúc</p>
                <p class="text-white font-semibold">
                  {{ formatDateTime(article.event_end_time) }}
                </p>
              </div>
            </div>
            <div class="flex items-center md:col-span-2">
              <i class="fas fa-map-marker-alt text-red-400 mr-3"></i>
              <div>
                <p class="text-gray-400 text-sm">Địa điểm</p>
                <p class="text-white font-semibold">
                  {{ article.event_location }}
                </p>
              </div>
            </div>
          </div>
          <div class="mt-6 text-center">
            <button class="btn btn-error text-white font-bold px-8 pulse-red">
              <i class="fas fa-ticket-alt mr-2"></i>Đăng ký tham gia
            </button>
          </div>
        </div>

        <!-- Social Share -->
        <div
          class="mt-12 bg-gray-900/50 rounded-lg p-6 border border-red-600/20"
        >
          <h3 class="text-xl font-bold text-red-400 mb-4">
            <i class="fas fa-share-alt mr-2"></i>Chia sẻ bài viết
          </h3>
          <div class="flex flex-wrap gap-4">
            <button
              class="btn btn-circle btn-outline border-blue-500 text-blue-400 hover:bg-blue-500 hover:text-white"
            >
              <i class="fab fa-facebook-f"></i>
            </button>
            <button
              class="btn btn-circle btn-outline border-blue-400 text-blue-400 hover:bg-blue-400"
            >
              <i class="fab fa-twitter"></i>
            </button>
            <button
              class="btn btn-circle btn-outline border-red-500 text-red-400 hover:bg-red-500 hover:text-white"
            >
              <i class="fab fa-instagram"></i>
            </button>
            <button
              class="btn btn-circle btn-outline border-red-600 text-red-400 hover:bg-red-600 hover:text-white"
            >
              <i class="fab fa-youtube"></i>
            </button>
            <button
              class="btn btn-circle btn-outline border-gray-500 text-gray-400 hover:bg-gray-500 hover:text-white"
            >
              <i class="fas fa-link"></i>
            </button>
          </div>
        </div>
      </div>
    </div>

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
                    relatedArticle.featured_image_url ||
                    '/images/placeholder.jpg'
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

    <!-- Comment Section -->
    <div class="container mx-auto px-4 py-16">
      <div class="max-w-4xl mx-auto">
        <h2 class="text-3xl font-bold mb-8 text-gradient">
          <i class="fas fa-comments mr-3"></i>Bình luận
        </h2>

        <!-- Comment Form -->
        <div
          class="bg-gray-900/50 rounded-lg p-6 border border-red-600/20 mb-8"
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
                  class="input input-bordered bg-gray-800 text-white border-gray-600 focus:border-red-500"
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
                  class="input input-bordered bg-gray-800 text-white border-gray-600 focus:border-red-500"
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
                class="textarea textarea-bordered bg-gray-800 text-white border-gray-600 focus:border-red-500 h-24"
                placeholder="Viết bình luận của bạn..."
                required
              ></textarea>
            </div>
            <div class="text-right">
              <button type="submit" class="btn btn-error text-white font-bold">
                <i class="fas fa-paper-plane mr-2"></i>Gửi bình luận
              </button>
            </div>
          </form>
        </div>

        <!-- Comments List -->
        <div class="space-y-6">
          <div
            v-for="comment in comments"
            :key="comment.id"
            class="bg-gray-800/30 rounded-lg p-6 border border-gray-700"
          >
            <div class="flex items-start space-x-4">
              <div
                class="w-12 h-12 bg-red-600 rounded-full flex items-center justify-center"
              >
                <i class="fas fa-user text-white"></i>
              </div>
              <div class="flex-1">
                <div class="flex items-center justify-between mb-2">
                  <h4 class="font-bold text-white">{{ comment.name }}</h4>
                  <span class="text-gray-500 text-sm">{{
                    formatDate(comment.created_at)
                  }}</span>
                </div>
                <p class="text-gray-300 leading-relaxed">
                  {{ comment.content }}
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

// Props
const route = useRoute();
const articleId = route.params.id;

// Reactive data
const article = ref({});
const relatedArticles = ref([]);
const comments = ref([]);
const newComment = ref({
  name: "",
  email: "",
  content: "",
});

// Mock data - Replace with actual API calls
const mockArticle = {
  id: 1,
  title: "Top 10 Bài Tập Gym Hiệu Quả Nhất Cho Người Mới Bắt Đầu",
  content: `
    <p>Bạn mới bắt đầu tập gym và không biết nên tập những bài tập nào? Đây là danh sách 10 bài tập gym cơ bản nhất mà bất kỳ ai cũng nên biết.</p>
    
    <h2>1. Squat - Vua của các bài tập</h2>
    <p>Squat là một trong những bài tập quan trọng nhất để phát triển sức mạnh và khối lượng cơ bắp toàn thân. Bài tập này chủ yếu tập trung vào cơ đùi, cơ mông và cơ core.</p>
    
    <h3>Cách thực hiện:</h3>
    <ul>
      <li>Đứng thẳng với hai chân mở rộng bằng vai</li>
      <li>Từ từ hạ thấp người xuống như ngồi ghế</li>
      <li>Giữ lưng thẳng, không cúi người về phía trước</li>
      <li>Bậu chân trở lại vị trí ban đầu</li>
    </ul>
    
    <h2>2. Deadlift - Bài tập toàn thân</h2>
    <p>Deadlift là bài tập giúp phát triển sức mạnh toàn thân, đặc biệt là cơ lưng dưới, cơ mông và cơ đùi sau.</p>
    
    <h2>3. Bench Press - Phát triển ngực</h2>
    <p>Bench press là bài tập cơ bản nhất để phát triển cơ ngực, cùng với cơ vai trước và cơ tricep.</p>
    
    <h2>4. Pull-up - Cường hóa lưng</h2>
    <p>Pull-up là bài tập tuyệt vời để phát triển cơ lưng, cơ bắp tay và cơ cẳng tay.</p>
    
    <h2>5. Overhead Press - Phát triển vai</h2>
    <p>Bài tập này giúp phát triển cơ vai, cải thiện sức mạnh và sự ổn định của phần thân trên.</p>
    
    <h2>Lưu ý khi tập luyện</h2>
    <p>Khi mới bắt đầu tập gym, điều quan trọng nhất là học đúng kỹ thuật. Hãy bắt đầu với trọng lượng nhẹ và tập trung vào form chuẩn trước khi tăng tải.</p>
    
    <p>Nếu bạn cần hỗ trợ, đừng ngại đăng ký gói tập với huấn luyện viên cá nhân tại PowerGym. Chúng tôi sẽ giúp bạn xây dựng chương trình tập luyện phù hợp và an toàn.</p>
  `,
  excerpt:
    "Khám phá 10 bài tập gym cơ bản nhất mà mọi người mới bắt đầu cần biết để có một khởi đầu tốt trong hành trình tập luyện.",
  featured_image_url: "/images/gym-workouts.jpg",
  type: "article",
  published_at: "2024-01-15T10:00:00Z",
  view_count: 1245,
  author: "Huấn luyện viên Nam",
  category: "Hướng dẫn tập luyện",
};

const mockRelatedArticles = [
  {
    id: 2,
    title: "Dinh dưỡng cho người tập gym",
    excerpt: "Hướng dẫn chế độ dinh dưỡng hợp lý cho người tập gym.",
    featured_image_url: "/images/nutrition.jpg",
    published_at: "2024-01-10T09:00:00Z",
    view_count: 856,
  },
  {
    id: 3,
    title: "Cách chọn giày tập gym phù hợp",
    excerpt: "Mẹo chọn giày tập gym để tránh chấn thương.",
    featured_image_url: "/images/gym-shoes.jpg",
    published_at: "2024-1-08T14:30:00Z",
    view_count: 432,
  },
  {
    id: 4,
    title: "Lỗi sai phổ biến khi tập gym",
    excerpt: "Những lỗi thường gặp và cách khắc phục.",
    featured_image_url: "/images/gym-mistakes.jpg",
    published_at: "2024-01-05T11:15:00Z",
    view_count: 789,
  },
];

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

const submitComment = () => {
  if (
    !newComment.value.name ||
    !newComment.value.email ||
    !newComment.value.content
  ) {
    alert("Vui lòng điền đầy đủ thông tin!");
    return;
  }

  // Here you would typically send the comment to your API
  const comment = {
    id: Date.now(),
    name: newComment.value.name,
    content: newComment.value.content,
    created_at: new Date().toISOString(),
  };

  comments.value.unshift(comment);
  newComment.value = { name: "", email: "", content: "" };

  // Show success message
  alert("Bình luận đã được gửi thành công!");
};

// Lifecycle
onMounted(() => {
  // In a real app, you would fetch the article data from your API
  // based on the articleId
  article.value = mockArticle;
  relatedArticles.value = mockRelatedArticles;
  comments.value = mockComments;
});

// SEO
useHead({
  title: () => `${article.value.title} | PowerGym`,
  meta: [
    {
      name: "description",
      content: () =>
        article.value.excerpt || "PowerGym - Phòm tập gym hàng đầu Việt Nam",
    },
    {
      name: "keywords",
      content: () =>
        article.value.meta_keywords || "gym, fitness, tập luyện, sức khỏe",
    },
  ],
});
</script>
