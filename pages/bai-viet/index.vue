<template>
  <div class="min-h-screen bg-black text-white">
    <!-- Hero Section -->
    <div class="bg-gradient-to-r from-red-900 to-black py-16 pt-24">
      <div class="container mx-auto px-4">
        <div class="text-center">
          <h1 class="text-4xl md:text-6xl font-bold mb-4">
            <span class="text-red-500">BÀI VIẾT</span> & TIN TỨC
          </h1>
          <p class="text-xl text-gray-300 max-w-2xl mx-auto">
            Cập nhật những thông tin mới nhất về thể hình, dinh dưỡng và các sự kiện tại AlphaGym
          </p>
        </div>
      </div>
    </div>

    <div class="container mx-auto px-4 py-12">
      <div class="grid grid-cols-1 lg:grid-cols-4 gap-8">
        <!-- Sidebar -->
        <div class="lg:col-span-1">
          <div class="bg-gray-900 rounded-lg p-6 sticky top-4">
            <h3 class="text-xl font-bold text-red-500 mb-4">DANH MỤC</h3>
            
            <!-- Loading categories -->
            <div v-if="categoriesLoading" class="space-y-2">
              <div v-for="n in 4" :key="n" class="animate-pulse">
                <div class="h-12 bg-gray-700 rounded-lg"></div>
              </div>
            </div>
            
            <!-- Categories list -->
            <div v-else class="space-y-2">
              <!-- All articles option -->
              <div
                @click="filterByCategory('')"
                class="flex items-center justify-between p-3 rounded-lg transition-colors group cursor-pointer"
                :class="selectedCategory === '' ? 'bg-red-900 text-white' : 'bg-gray-800 hover:bg-red-900'"
              >
                <span class="group-hover:text-white">Tất cả bài viết</span>
                <span class="bg-red-500 text-white text-xs px-2 py-1 rounded-full">
                  {{ totalArticlesCount }}
                </span>
              </div>
              
              <!-- Category items -->
              <div
                v-for="category in categories"
                :key="category.category_id"
                @click="filterByCategory(category.slug)"
                class="flex items-center justify-between p-3 rounded-lg transition-colors group cursor-pointer"
                :class="selectedCategory === category.slug ? 'bg-red-900 text-white' : 'bg-gray-800 hover:bg-red-900'"
              >
                <span class="group-hover:text-white">{{ category.name }}</span>
                <span class="bg-red-500 text-white text-xs px-2 py-1 rounded-full">
                  {{ category.articles_count }}
                </span>
              </div>
            </div>

            <!-- Search Box -->
            <div class="mt-6">
              <h3 class="text-xl font-bold text-red-500 mb-4">TÌM KIẾM</h3>
              <input
                v-model="searchQuery"
                type="text"
                placeholder="Tìm kiếm bài viết..."
                class="w-full px-4 py-2 rounded-lg bg-gray-800 text-white border border-gray-700 focus:border-red-500 focus:outline-none"
                @input="searchArticles"
              />
            </div>
          </div>
        </div>

        <!-- Articles Grid -->
        <div class="lg:col-span-3">
          <!-- Loading State -->
          <div v-if="articlesLoading" class="grid grid-cols-1 md:grid-cols-2 gap-8">
            <div v-for="n in 6" :key="n" class="bg-gray-900 rounded-lg overflow-hidden animate-pulse">
              <div class="h-48 bg-gray-700"></div>
              <div class="p-6">
                <div class="h-4 bg-gray-700 rounded mb-2"></div>
                <div class="h-6 bg-gray-700 rounded mb-3"></div>
                <div class="h-16 bg-gray-700 rounded"></div>
              </div>
            </div>
          </div>

          <!-- Error State -->
          <div v-else-if="articlesError" class="text-center py-12">
            <div class="text-6xl mb-4 text-red-500">
              <i class="fas fa-exclamation-triangle"></i>
            </div>
            <h3 class="text-2xl font-bold text-gray-200 mb-3">Có lỗi xảy ra</h3>
            <p class="text-gray-400">{{ articlesError.message || 'Không thể tải bài viết' }}</p>
            <button @click="refreshArticles" class="mt-4 bg-red-500 hover:bg-red-600 text-white px-6 py-2 rounded-lg transition-colors">
              Thử lại
            </button>
          </div>

          <!-- Articles -->
          <div v-else class="grid grid-cols-1 md:grid-cols-2 gap-8">
            <div
              v-for="article in filteredArticles"
              :key="article.article_id"
              class="bg-gray-900 rounded-lg overflow-hidden hover:transform hover:scale-105 transition-all duration-300 group cursor-pointer"
              @click="viewArticle(article.slug)"
            >
              <div class="relative">
                <img
                  :src="article.image || 'https://placehold.co/400x300/333333/EFEFEF?text=No+Image' "
                  :alt="article.title"
                  class="w-full h-48 object-cover group-hover:opacity-80 transition-opacity"
                  @error="handleImageError"
                />
                <div class="absolute top-4 left-4">
                  <span class="bg-red-500 text-white px-3 py-1 rounded-full text-sm font-semibold">
                    {{ getCategoryName(article) }}
                  </span>
                </div>
                <div class="absolute top-4 right-4">
                  <span class="bg-black bg-opacity-70 text-white px-2 py-1 rounded text-xs">
                    {{ getArticleType(article.type) }}
                  </span>
                </div>
              </div>

              <div class="p-6">
                <div class="flex items-center text-gray-400 text-sm mb-3">
                  <svg class="w-4 h-4 mr-2" fill="currentColor" viewBox="0 0 20 20">
                    <path
                      fill-rule="evenodd"
                      d="M6 2a1 1 0 00-1 1v1H4a2 2 0 00-2 2v10a2 2 0 002 2h12a2 2 0 002-2V6a2 2 0 00-2-2h-1V3a1 1 0 10-2 0v1H7V3a1 1 0 00-1-1zm0 5a1 1 0 000 2h8a1 1 0 100-2H6z"
                      clip-rule="evenodd"
                    />
                  </svg>
                  {{ formatDate(article.published_at) }}
                  <span class="mx-2">•</span>
                  <svg class="w-4 h-4 mr-1" fill="currentColor" viewBox="0 0 20 20">
                    <path d="M10 12a2 2 0 100-4 2 2 0 000 4z" />
                    <path
                      fill-rule="evenodd"
                      d="M.458 10C1.732 5.943 5.522 3 10 3s8.268 2.943 9.542 7c-1.274 4.057-5.064 7-9.542 7S1.732 14.057.458 10zM14 10a4 4 0 11-8 0 4 4 0 018 0z"
                      clip-rule="evenodd"
                    />
                  </svg>
                  {{ article.view_count || 0 }} lượt xem
                </div>

                <h2 class="text-xl font-bold mb-3 group-hover:text-red-500 transition-colors line-clamp-2">
                  {{ article.title }}
                </h2>

                <p class="text-gray-300 mb-4 line-clamp-3">{{ article.excerpt || 'Không có mô tả' }}</p>

                <div class="inline-flex items-center text-red-500 hover:text-red-400 font-semibold">
                  Đọc thêm
                  <svg class="w-4 h-4 ml-2" fill="currentColor" viewBox="0 0 20 20">
                    <path
                      fill-rule="evenodd"
                      d="M10.293 3.293a1 1 0 011.414 0l6 6a1 1 0 010 1.414l-6 6a1 1 0 01-1.414-1.414L14.586 11H3a1 1 0 110-2h11.586l-4.293-4.293a1 1 0 010-1.414z"
                      clip-rule="evenodd"
                    />
                  </svg>
                </div>
              </div>
            </div>
          </div>

          <!-- No Results -->
          <div v-if="!articlesLoading && !articlesError && filteredArticles.length === 0" class="text-center py-12">
            <svg class="w-16 h-16 text-gray-600 mx-auto mb-4" fill="currentColor" viewBox="0 0 20 20">
              <path fill-rule="evenodd" d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z" clip-rule="evenodd" />
            </svg>
            <h3 class="text-xl font-semibold text-gray-400 mb-2">Không tìm thấy bài viết</h3>
            <p class="text-gray-500">Thử tìm kiếm với từ khóa khác hoặc xem tất cả bài viết</p>
            <button @click="clearFilters" class="mt-4 bg-red-500 hover:bg-red-600 text-white px-6 py-2 rounded-lg transition-colors">
              Xem tất cả
            </button>
          </div>

          <!-- Pagination -->
          <div v-if="!articlesLoading && !articlesError && filteredArticles.length > 0 && totalPages > 1" class="flex justify-center mt-12">
            <div class="join">
              <button 
                class="join-item btn btn-outline border-red-500 text-red-500 hover:bg-red-500 hover:text-white"
                :disabled="currentPage === 1"
                @click="changePage(currentPage - 1)"
              >
                «
              </button>
              <button 
                v-for="page in visiblePages" 
                :key="page"
                class="join-item btn"
                :class="page === currentPage ? 'bg-red-500 text-white' : 'btn-outline border-red-500 text-red-500 hover:bg-red-500 hover:text-white'"
                @click="changePage(page)"
              >
                {{ page }}
              </button>
              <button 
                class="join-item btn btn-outline border-red-500 text-red-500 hover:bg-red-500 hover:text-white"
                :disabled="currentPage === totalPages"
                @click="changePage(currentPage + 1)"
              >
                »
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
import { useRuntimeConfig, useFetch, navigateTo } from '#app'


// Configuration
const config = useRuntimeConfig()
const articlesPerPage = 6

// Reactive data
const searchQuery = ref('')
const selectedCategory = ref('')
const currentPage = ref(1)

// API calls
const { data: articlesResponse, pending: articlesLoading, error: articlesError, refresh: refreshArticles } = await useFetch(
  `${config.public.apiBase}/articles`
)

const { data: categoriesResponse, pending: categoriesLoading, error: categoriesError } = await useFetch(
  `${config.public.apiBase}/article-categories`
)

// Computed properties
const articles = computed(() => {
  if (articlesResponse.value && articlesResponse.value.data) {
    return articlesResponse.value.data
  }
  return []
})

const categories = computed(() => {
  if (categoriesResponse.value && categoriesResponse.value.data) {
    return categoriesResponse.value.data
  }
  return []
})

const totalArticlesCount = computed(() => articles.value.length)

const filteredArticles = computed(() => {
  let filtered = articles.value

  // Filter by category
  if (selectedCategory.value) {
    filtered = filtered.filter(article => {
      // Check if article has category relationship
      if (article.category && article.category.slug === selectedCategory.value) {
        return true
      }
      // Fallback to article_category_id matching
      const category = categories.value.find(cat => cat.slug === selectedCategory.value)
      return category && article.article_category_id === category.category_id
    })
  }

  // Filter by search query
  if (searchQuery.value) {
    const query = searchQuery.value.toLowerCase()
    filtered = filtered.filter(article => 
      article.title.toLowerCase().includes(query) ||
      (article.excerpt && article.excerpt.toLowerCase().includes(query)) ||
      (article.content && article.content.toLowerCase().includes(query))
    )
  }

  // Pagination
  const start = (currentPage.value - 1) * articlesPerPage
  const end = start + articlesPerPage
  return filtered.slice(start, end)
})

const totalFilteredCount = computed(() => {
  let filtered = articles.value

  if (selectedCategory.value) {
    filtered = filtered.filter(article => {
      if (article.category && article.category.slug === selectedCategory.value) {
        return true
      }
      const category = categories.value.find(cat => cat.slug === selectedCategory.value)
      return category && article.article_category_id === category.category_id
    })
  }

  if (searchQuery.value) {
    const query = searchQuery.value.toLowerCase()
    filtered = filtered.filter(article => 
      article.title.toLowerCase().includes(query) ||
      (article.excerpt && article.excerpt.toLowerCase().includes(query)) ||
      (article.content && article.content.toLowerCase().includes(query))
    )
  }

  return filtered.length
})

const totalPages = computed(() => {
  return Math.ceil(totalFilteredCount.value / articlesPerPage)
})

const visiblePages = computed(() => {
  const pages = []
  const maxVisible = 5
  let start = Math.max(1, currentPage.value - Math.floor(maxVisible / 2))
  let end = Math.min(totalPages.value, start + maxVisible - 1)
  
  if (end - start + 1 < maxVisible) {
    start = Math.max(1, end - maxVisible + 1)
  }
  
  for (let i = start; i <= end; i++) {
    pages.push(i)
  }
  
  return pages
})

// Methods
const formatDate = (dateString) => {
  if (!dateString) return 'Chưa cập nhật'
  return new Date(dateString).toLocaleDateString('vi-VN', {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  })
}

const getCategoryName = (article) => {
  // Try to get category name from relationship first
  if (article.category && article.category.name) {
    return article.category.name
  }
  
  // Fallback to finding category by ID
  if (article.article_category_id) {
    const category = categories.value.find(cat => cat.category_id === article.article_category_id)
    if (category) {
      return category.name
    }
  }
  
  return 'Khác'
}

const getArticleType = (type) => {
  const typeMap = {
    'news': 'Tin tức',
    'event': 'Sự kiện',
    'blog': 'Blog',
    'promotion': 'Khuyến mãi'
  }
  return typeMap[type] || 'Bài viết'
}

const getImageUrl = (imageUrl) => {
  if (!imageUrl) {
    return '/placeholder.svg?height=300&width=400'
  }
  
  // If it's already a full URL, return as is
  if (imageUrl.startsWith('http')) {
    return imageUrl
  }
  
  // If it's a relative path, prepend the base URL
  return `${config.public.apiBase}/${imageUrl}`
}

const handleImageError = (event) => {
  event.target.src = '/placeholder.svg?height=300&width=400'
}

const filterByCategory = (categorySlug) => {
  selectedCategory.value = selectedCategory.value === categorySlug ? '' : categorySlug
  currentPage.value = 1
}

const searchArticles = () => {
  currentPage.value = 1
}

const clearFilters = () => {
  searchQuery.value = ''
  selectedCategory.value = ''
  currentPage.value = 1
}

const changePage = (page) => {
  if (page >= 1 && page <= totalPages.value) {
    currentPage.value = page
    // Scroll to top when changing page
    window.scrollTo({ top: 0, behavior: 'smooth' })
  }
}

const viewArticle = (slug) => {
  navigateTo(`/bai-viet/${slug}`)
}

// Watch for changes in filters to reset pagination
watch([searchQuery, selectedCategory], () => {
  currentPage.value = 1
})

// SEO
useHead({
  title: 'Bài viết & Tin tức | AlphaGym',
  meta: [
    {
      name: 'description',
      content: 'Cập nhật những thông tin mới nhất về thể hình, dinh dưỡng và các sự kiện tại AlphaGym'
    },
    {
      name: 'keywords',
      content: 'gym, fitness, thể hình, tin tức, bài viết, sự kiện, khuyến mãi'
    }
  ]
})
</script>

<style scoped>
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
</style>