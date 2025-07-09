<template>
  <div class="min-h-screen bg-black text-white">
    <!-- Hero Section -->
    <div
      class="relative py-20 bg-gradient-to-r from-red-900 via-red-800 to-black"
    >
      <div class="container mx-auto px-4">
        <div class="max-w-4xl mx-auto text-center">
          <h1 class="text-5xl md:text-6xl font-bold mb-6 text-gradient">
            <i class="fas fa-newspaper mr-4"></i>Tin Tức & Bài Viết
          </h1>
          <p class="text-xl text-gray-300 mb-8">
            Cập nhật những thông tin mới nhất về thể hình, dinh dưỡng và sức
            khỏe
          </p>

          <!-- Search Bar -->
          <div class="relative max-w-2xl mx-auto">
            <input
              type="text"
              v-model="searchTerm"
              @input="filterArticles"
              placeholder="Tìm kiếm bài viết..."
              class="input input-bordered input-lg w-full bg-black/50 text-white border-red-600/50 focus:border-red-500 pr-16"
            />
            <button
              class="absolute right-2 top-1/2 transform -translate-y-1/2 btn btn-error btn-sm"
            >
              <i class="fas fa-search"></i>
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Filters & Controls -->
    <div class="bg-gray-900/50 border-b border-red-600/30">
      <div class="container mx-auto px-4 py-6">
        <div class="flex flex-wrap items-center justify-between gap-4">
          <!-- Categories Filter -->
          <div class="flex flex-wrap items-center gap-2">
            <span class="text-gray-300 mr-2">Danh mục:</span>
            <div class="tabs tabs-boxed bg-gray-800/50">
              <a
                v-for="category in categories"
                :key="category.value"
                :class="[
                  'tab',
                  selectedCategory === category.value
                    ? 'tab-active bg-red-600 text-white'
                    : 'text-gray-300 hover:text-red-400',
                ]"
                @click="filterByCategory(category.value)"
              >
                {{ category.label }}
              </a>
            </div>
          </div>

          <!-- View Controls -->
          <div class="flex items-center gap-4">
            <!-- Sort -->
            <div class="dropdown dropdown-end">
              <label
                tabindex="0"
                class="btn btn-outline btn-sm border-red-600/50 text-red-400 hover:bg-red-600 hover:text-white"
              >
                <i class="fas fa-sort mr-2"></i>{{ getSortLabel() }}
              </label>
              <ul
                tabindex="0"
                class="dropdown-content z-[1] menu p-2 shadow bg-gray-800 rounded-box w-52"
              >
                <li>
                  <a
                    @click="sortArticles('newest')"
                    class="text-white hover:text-red-400"
                    >Mới nhất</a
                  >
                </li>
                <li>
                  <a
                    @click="sortArticles('oldest')"
                    class="text-white hover:text-red-400"
                    >Cũ nhất</a
                  >
                </li>
                <li>
                  <a
                    @click="sortArticles('popular')"
                    class="text-white hover:text-red-400"
                    >Phổ biến</a
                  >
                </li>
                <li>
                  <a
                    @click="sortArticles('title')"
                    class="text-white hover:text-red-400"
                    >Tên A-Z</a
                  >
                </li>
              </ul>
            </div>

            <!-- View Mode -->
            <div class="btn-group">
              <button
                :class="[
                  'btn btn-sm',
                  viewMode === 'grid'
                    ? 'btn-error'
                    : 'btn-outline border-red-600/50 text-red-400 hover:bg-red-600 hover:text-white',
                ]"
                @click="viewMode = 'grid'"
              >
                <i class="fas fa-th"></i>
              </button>
              <button
                :class="[
                  'btn btn-sm',
                  viewMode === 'list'
                    ? 'btn-error'
                    : 'btn-outline border-red-600/50 text-red-400 hover:bg-red-600 hover:text-white',
                ]"
                @click="viewMode = 'list'"
              >
                <i class="fas fa-list"></i>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Articles Content -->
    <div class="container mx-auto px-4 py-16">
      <!-- Loading State -->
      <div v-if="isLoading" class="flex justify-center items-center py-20">
        <div class="loading loading-spinner loading-lg text-red-500"></div>
      </div>

      <!-- Empty State -->
      <div v-else-if="currentArticles.length === 0" class="text-center py-20">
        <div class="text-6xl mb-4">
          <i class="fas fa-search text-red-400"></i>
        </div>
        <h3 class="text-2xl font-bold text-gray-300 mb-2">
          Không tìm thấy bài viết
        </h3>
        <p class="text-gray-500">Thử thay đổi từ khóa tìm kiếm hoặc bộ lọc</p>
      </div>

      <!-- Grid View -->
      <div
        v-else-if="viewMode === 'grid'"
        class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8"
      >
        <div
          v-for="article in currentArticles"
          :key="article.id"
          class="card bg-gray-800/50 border border-red-600/20 card-hover cursor-pointer group"
          @click="navigateToArticle(article.id)"
        >
          <!-- Article Image -->
          <figure class="relative overflow-hidden">
            <img
              :src="article.featured_image_url || '/images/placeholder.jpg'"
              :alt="article.title"
              class="w-full h-48 object-cover transition-transform duration-300 group-hover:scale-110"
            />
            <div class="absolute top-4 left-4">
              <div class="badge badge-error text-white font-bold">
                {{ article.category }}
              </div>
            </div>
            <div v-if="article.type === 'event'" class="absolute top-4 right-4">
              <div class="badge badge-warning text-black font-bold">
                <i class="fas fa-calendar mr-1"></i>Sự kiện
              </div>
            </div>
          </figure>

          <!-- Article Content -->
          <div class="card-body">
            <h2
              class="card-title text-white text-lg line-clamp-2 group-hover:text-red-400 transition-colors"
            >
              {{ article.title }}
            </h2>
            <p class="text-gray-400 text-sm line-clamp-3 mb-4">
              {{ article.excerpt }}
            </p>

            <!-- Article Meta -->
            <div
              class="flex items-center justify-between text-gray-500 text-sm mb-4"
            >
              <div class="flex items-center space-x-4">
                <div class="flex items-center">
                  <i class="fas fa-user mr-1"></i>
                  <span>{{ article.author || "PowerGym" }}</span>
                </div>
                <div class="flex items-center">
                  <i class="fas fa-eye mr-1"></i>
                  <span>{{ formatViewCount(article.view_count) }}</span>
                </div>
              </div>
              <div class="flex items-center">
                <i class="fas fa-calendar mr-1"></i>
                <span>{{ formatDate(article.published_at) }}</span>
              </div>
            </div>

            <!-- Action -->
            <div class="card-actions justify-end">
              <button
                class="btn btn-error btn-sm text-white group-hover:btn-outline group-hover:border-red-500 group-hover:text-red-400"
              >
                Xem thêm
                <i class="fas fa-arrow-right ml-2"></i>
              </button>
            </div>
          </div>
        </div>
      </div>

      <!-- List View -->
      <div v-else class="space-y-6">
        <div
          v-for="article in currentArticles"
          :key="article.id"
          class="card lg:card-side bg-gray-800/50 border border-red-600/20 card-hover cursor-pointer group"
          @click="navigateToArticle(article.id)"
        >
          <figure class="lg:w-80">
            <img
              :src="article.featured_image_url || '/images/placeholder.jpg'"
              :alt="article.title"
              class="w-full h-48 lg:h-full object-cover transition-transform duration-300 group-hover:scale-105"
            />
          </figure>
          <div class="card-body">
            <div class="flex flex-wrap items-center gap-2 mb-2">
              <div class="badge badge-error text-white font-bold">
                {{ article.category }}
              </div>
              <div
                v-if="article.type === 'event'"
                class="badge badge-warning text-black font-bold"
              >
                <i class="fas fa-calendar mr-1"></i>Sự kiện
              </div>
            </div>

            <h2
              class="card-title text-white text-xl mb-3 group-hover:text-red-400 transition-colors"
            >
              {{ article.title }}
            </h2>

            <p class="text-gray-400 line-clamp-3 mb-4">
              {{ article.excerpt }}
            </p>

            <div
              class="flex items-center justify-between text-gray-500 text-sm mb-4"
            >
              <div class="flex items-center space-x-6">
                <div class="flex items-center">
                  <i class="fas fa-user mr-2"></i>
                  <span>{{ article.author || "PowerGym" }}</span>
                </div>
                <div class="flex items-center">
                  <i class="fas fa-eye mr-2"></i>
                  <span>{{ formatViewCount(article.view_count) }}</span>
                </div>
                <div class="flex items-center">
                  <i class="fas fa-calendar mr-2"></i>
                  <span>{{ formatDate(article.published_at) }}</span>
                </div>
              </div>
            </div>

            <div class="card-actions justify-end">
              <button
                class="btn btn-error text-white group-hover:btn-outline group-hover:border-red-500 group-hover:text-red-400"
              >
                Đọc tiếp
                <i class="fas fa-arrow-right ml-2"></i>
              </button>
            </div>
          </div>
        </div>
      </div>

      <!-- Pagination -->
      <div class="flex justify-center mt-12" v-if="totalPages > 1">
        <div class="btn-group">
          <button
            :disabled="currentPage === 1"
            @click="changePage(currentPage - 1)"
            class="btn btn-outline border-red-600/50 text-red-400 hover:bg-red-600 hover:text-white"
          >
            <i class="fas fa-chevron-left"></i>
          </button>

          <button
            v-for="page in visiblePages"
            :key="page"
            :class="[
              'btn',
              page === currentPage
                ? 'btn-error'
                : 'btn-outline border-red-600/50 text-red-400 hover:bg-red-600 hover:text-white',
            ]"
            @click="changePage(page)"
          >
            {{ page }}
          </button>

          <button
            :disabled="currentPage === totalPages"
            @click="changePage(currentPage + 1)"
            class="btn btn-outline border-red-600/50 text-red-400 hover:bg-red-600 hover:text-white"
          >
            <i class="fas fa-chevron-right"></i>
          </button>
        </div>
      </div>
    </div>

    <!-- Newsletter Subscription -->
    <div
      class="bg-gradient-to-r from-red-900/30 to-red-800/30 border-t border-red-600/30"
    >
      <div class="container mx-auto px-4 py-16">
        <div class="max-w-2xl mx-auto text-center">
          <h2 class="text-3xl font-bold mb-4 text-gradient">
            <i class="fas fa-bell mr-3"></i>Nhận thông báo mới
          </h2>
          <p class="text-gray-300 mb-8">
            Đăng ký để nhận những bài viết mới nhất về thể hình và sức khỏe
          </p>
          <div class="flex flex-col sm:flex-row gap-4 max-w-md mx-auto">
            <input
              type="email"
              placeholder="Nhập email của bạn"
              class="input input-bordered flex-1 bg-black/50 text-white border-red-600/50 focus:border-red-500"
            />
            <button class="btn btn-error text-white font-bold pulse-red">
              <i class="fas fa-paper-plane mr-2"></i>Đăng ký
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";

// Reactive data
const articles = ref([]);
const searchTerm = ref("");
const selectedCategory = ref("all");
const viewMode = ref("grid");
const sortBy = ref("newest");
const currentPage = ref(1);
const isLoading = ref(true);
const articlesPerPage = 9;

// Categories
const categories = ref([
  { value: "all", label: "Tất cả" },
  { value: "workout", label: "Hướng dẫn tập luyện" },
  { value: "nutrition", label: "Dinh dưỡng" },
  { value: "health", label: "Sức khỏe" },
  { value: "equipment", label: "Thiết bị" },
  { value: "news", label: "Tin tức" },
  { value: "event", label: "Sự kiện" },
]);

// Mock data
const mockArticles = [
  {
    id: 1,
    title: "Top 10 Bài Tập Gym Hiệu Quả Nhất Cho Người Mới Bắt Đầu",
    excerpt:
      "Khám phá 10 bài tập gym cơ bản nhất mà mọi người mới bắt đầu cần biết để có một khởi đầu tốt trong hành trình tập luyện.",
    featured_image_url:
      "https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=500&h=300&fit=crop",
    published_at: "2024-01-15T10:00:00Z",
    view_count: 1245,
    author: "Huấn luyện viên Nam",
    category: "workout",
    type: "article",
  },
  {
    id: 2,
    title: "Dinh Dưỡng Cho Người Tập Gym: Hướng Dẫn Chi Tiết",
    excerpt:
      "Tìm hiểu cách xây dựng chế độ dinh dưỡng hợp lý để tối ưu hóa kết quả tập luyện và phục hồi cơ bắp.",
    featured_image_url:
      "https://images.unsplash.com/photo-1490645935967-10de6ba17061?w=500&h=300&fit=crop",
    published_at: "2024-01-12T14:30:00Z",
    view_count: 856,
    author: "Chuyên gia dinh dưỡng Linh",
    category: "nutrition",
    type: "article",
  },
  {
    id: 3,
    title: "Sự Kiện Thi Đấu Powerlifting Tháng 2/2024",
    excerpt:
      "Tham gia giải thi đấu powerlifting lớn nhất năm tại PowerGym. Đăng ký ngay để thể hiện sức mạnh của bạn!",
    featured_image_url:
      "https://images.unsplash.com/photo-1581009146145-b5ef050c2e1e?w=500&h=300&fit=crop",
    published_at: "2024-01-10T09:00:00Z",
    view_count: 432,
    author: "Ban tổ chức",
    category: "event",
    type: "event",
    event_start_time: "2024-02-15T08:00:00Z",
    event_end_time: "2024-02-15T18:00:00Z",
    event_location: "PowerGym - Cơ sở chính",
  },
  {
    id: 4,
    title: "Cách Chọn Giày Tập Gym Phù Hợp Với Từng Loại Bài Tập",
    excerpt:
      "Hướng dẫn chi tiết cách chọn giày tập gym phù hợp để tăng hiệu quả tập luyện và tránh chấn thương.",
    featured_image_url:
      "https://images.unsplash.com/photo-1600185365483-26d7a4cc7519?w=500&h=300&fit=crop",
    published_at: "2024-01-08T16:45:00Z",
    view_count: 789,
    author: "Huấn luyện viên Minh",
    category: "equipment",
    type: "article",
  },
  {
    id: 5,
    title: "Lợi Ích Của Việc Tập Gym Đối Với Sức Khỏe Tinh Thần",
    excerpt:
      "Khám phá những tác động tích cực của việc tập gym đối với sức khỏe tinh thần và chất lượng cuộc sống.",
    featured_image_url:
      "https://images.unsplash.com/photo-1599058917212-d750089bc07e?w=500&h=300&fit=crop",
    published_at: "2024-01-06T11:20:00Z",
    view_count: 1123,
    author: "Bác sĩ Hương",
    category: "health",
    type: "article",
  },
  {
    id: 6,
    title: "PowerGym Khai Trương Cơ Sở Mới Tại Quận 7",
    excerpt:
      "Chúng tôi tự hao mừng thông báo về việc khai trương cơ sở mới với trang thiết bị hiện đại nhất.",
    featured_image_url:
      "https://images.unsplash.com/photo-1534438327276-14e5300c3a48?w=500&h=300&fit=crop",
    published_at: "2024-01-04T08:00:00Z",
    view_count: 2156,
    author: "PowerGym Team",
    category: "news",
    type: "article",
  },
  {
    id: 7,
    title: "Bí Quyết Tăng Cường Sức Mạnh Với Deadlift",
    excerpt:
      "Hướng dẫn kỹ thuật thực hiện deadlift chuẩn và các biến thể để tăng cường sức mạnh toàn thân.",
    featured_image_url:
      "https://images.unsplash.com/photo-1583500178690-f7ff6d1d4215?w=500&h=300&fit=crop",
    published_at: "2024-01-02T13:15:00Z",
    view_count: 967,
    author: "Huấn luyện viên Tuấn",
    category: "workout",
    type: "article",
  },
  {
    id: 8,
    title: "Thực Đơn Tăng Cơ Trong 30 Ngày",
    excerpt:
      "Thực đơn chi tiết cho 30 ngày giúp bạn tăng cơ hiệu quả kết hợp với chế độ tập luyện phù hợp.",
    featured_image_url:
      "https://images.unsplash.com/photo-1547592180-85f173990554?w=500&h=300&fit=crop",
    published_at: "2023-12-30T10:30:00Z",
    view_count: 1634,
    author: "Chuyên gia dinh dưỡng An",
    category: "nutrition",
    type: "article",
  },
  {
    id: 9,
    title: "Lời Khuyên Từ Chuyên Gia: Tránh Chấn Thương Khi Tập Gym",
    excerpt:
      "Những lời khuyên quan trọng từ chuyên gia về cách tránh chấn thương và tập luyện an toàn.",
    featured_image_url:
      "https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=500&h=300&fit=crop",
    published_at: "2023-12-28T15:45:00Z",
    view_count: 1089,
    author: "Bác sĩ Thể thao Khang",
    category: "health",
    type: "article",
  },
  {
    id: 10,
    title: "Trang Thiết Bị Mới Nhất Tại PowerGym 2024",
    excerpt:
      "Cập nhật về những trang thiết bị tập luyện mới nhất được đầu tư tại PowerGym trong năm 2024.",
    featured_image_url:
      "https://images.unsplash.com/photo-1517836357463-d25dfeac3438?w=500&h=300&fit=crop",
    published_at: "2023-12-26T09:00:00Z",
    view_count: 756,
    author: "Quản lý cơ sở",
    category: "equipment",
    type: "article",
  },
];

// Computed properties
const filteredArticles = computed(() => {
  let filtered = articles.value;

  // Filter by search term
  if (searchTerm.value) {
    filtered = filtered.filter(
      (article) =>
        article.title.toLowerCase().includes(searchTerm.value.toLowerCase()) ||
        article.excerpt.toLowerCase().includes(searchTerm.value.toLowerCase())
    );
  }

  // Filter by category
  if (selectedCategory.value !== "all") {
    filtered = filtered.filter(
      (article) => article.category === selectedCategory.value
    );
  }

  // Sort articles
  switch (sortBy.value) {
    case "newest":
      filtered.sort(
        (a, b) => new Date(b.published_at) - new Date(a.published_at)
      );
      break;
    case "oldest":
      filtered.sort(
        (a, b) => new Date(a.published_at) - new Date(b.published_at)
      );
      break;
    case "popular":
      filtered.sort((a, b) => b.view_count - a.view_count);
      break;
    case "title":
      filtered.sort((a, b) => a.title.localeCompare(b.title));
      break;
  }

  return filtered;
});

const totalPages = computed(() =>
  Math.ceil(filteredArticles.value.length / articlesPerPage)
);

const currentArticles = computed(() => {
  const start = (currentPage.value - 1) * articlesPerPage;
  const end = start + articlesPerPage;
  return filteredArticles.value.slice(start, end);
});

const visiblePages = computed(() => {
  const pages = [];
  const total = totalPages.value;
  const current = currentPage.value;

  if (total <= 7) {
    for (let i = 1; i <= total; i++) {
      pages.push(i);
    }
  } else {
    if (current <= 4) {
      for (let i = 1; i <= 5; i++) {
        pages.push(i);
      }
      pages.push("...", total);
    } else if (current >= total - 3) {
      pages.push(1, "...");
      for (let i = total - 4; i <= total; i++) {
        pages.push(i);
      }
    } else {
      pages.push(1, "...");
      for (let i = current - 1; i <= current + 1; i++) {
        pages.push(i);
      }
      pages.push("...", total);
    }
  }

  return pages;
});

// Methods
const filterArticles = () => {
  currentPage.value = 1;
};

const filterByCategory = (category) => {
  selectedCategory.value = category;
  currentPage.value = 1;
};

const sortArticles = (sortType) => {
  sortBy.value = sortType;
  currentPage.value = 1;
};

const changePage = (page) => {
  if (page >= 1 && page <= totalPages.value) {
    currentPage.value = page;
    // Scroll to top
    window.scrollTo({ top: 0, behavior: "smooth" });
  }
};

const navigateToArticle = (id) => {
  // Navigate to article detail page
  window.location.href = `/article/${id}`;
};

const formatDate = (dateString) => {
  if (!dateString) return "Chưa cập nhật";

  const date = new Date(dateString);
  const now = new Date();
  const diffTime = Math.abs(now - date);
  const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));

  if (diffDays === 1) return "Hôm qua";
  if (diffDays < 7) return `${diffDays} ngày trước`;
  if (diffDays < 30) return `${Math.ceil(diffDays / 7)} tuần trước`;

  return date.toLocaleDateString("vi-VN", {
    year: "numeric",
    month: "long",
    day: "numeric",
  });
};

const formatViewCount = (count) => {
  if (!count) return "0";
  if (count < 1000) return count.toString();
  if (count < 1000000) return `${(count / 1000).toFixed(1)}K`;
  return `${(count / 1000000).toFixed(1)}M`;
};

const getSortLabel = () => {
  switch (sortBy.value) {
    case "newest":
      return "Mới nhất";
    case "oldest":
      return "Cũ nhất";
    case "popular":
      return "Phổ biến";
    case "title":
      return "Tên A-Z";
    default:
      return "Sắp xếp";
  }
};

const getCategoryLabel = (categoryValue) => {
  const category = categories.value.find((cat) => cat.value === categoryValue);
  return category ? category.label : "Khác";
};

// Lifecycle
onMounted(() => {
  // Simulate API call
  setTimeout(() => {
    articles.value = mockArticles;
    isLoading.value = false;
  }, 1000);
});

// SEO
useHead({
  title: "Tin tức & Bài viết | PowerGym",
  meta: [
    {
      name: "description",
      content:
        "Cập nhật những thông tin mới nhất về thể hình, dinh dưỡng và sức khỏe tại PowerGym. Khám phá các bài viết hữu ích từ đội ngũ chuyên gia.",
    },
    {
      name: "keywords",
      content:
        "tin tức gym, bài viết thể hình, hướng dẫn tập gym, dinh dưỡng thể thao, sức khỏe, PowerGym",
    },
  ],
});
</script>
