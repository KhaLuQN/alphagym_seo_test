<template>
  <div class="min-h-screen bg-black text-white">
    <!-- Hero Section -->
    <section class="bg-gradient-to-br from-gray-950 via-red-950 to-black py-24 md:py-32 overflow-hidden">
      <div class="container mx-auto px-6 relative z-10">
        <div class="text-center">
          <h1 class="text-5xl md:text-7xl font-extrabold mb-4 text-red-500 custom-hero-text-shadow leading-tight">
            <span class="text-white">Đội Ngũ</span>
            <span class="text-red-500"> Huấn Luyện Viên</span>
          </h1>
          <p class="text-xl md:text-2xl text-gray-300 max-w-2xl mx-auto">
            Gặp gỡ đội ngũ huấn luyện viên chuyên nghiệp với kinh nghiệm nhiều
            năm tại AlphaGym
          </p>
        </div>
      </div>
    </section>

    <!-- Filters Section -->
    <section class="bg-gray-900 py-8 border-b border-red-700/50 shadow-inner shadow-red-900/20">
      <div class="container mx-auto px-6">
        <div class="flex flex-wrap gap-4 justify-center">
          <button
            @click="selectedCategory = 'all'"
            :class="[
              'btn text-base font-medium px-6 py-3 rounded-full transition-all duration-300',
              selectedCategory === 'all'
                ? 'btn-error bg-red-600 text-white shadow-inner shadow-red-900/50'
                : 'btn-outline border-red-600 text-red-400 hover:bg-red-600 hover:text-white',
            ]"
          >
            Tất cả
          </button>
          <button
            @click="selectedCategory = 'fitness'"
            :class="[
              'btn text-base font-medium px-6 py-3 rounded-full transition-all duration-300',
              selectedCategory === 'fitness'
                ? 'btn-error bg-red-600 text-white shadow-inner shadow-red-900/50'
                : 'btn-outline border-red-600 text-red-400 hover:bg-red-600 hover:text-white',
            ]"
          >
            Fitness
          </button>
          <button
            @click="selectedCategory = 'yoga'"
            :class="[
              'btn text-base font-medium px-6 py-3 rounded-full transition-all duration-300',
              selectedCategory === 'yoga'
                ? 'btn-error bg-red-600 text-white shadow-inner shadow-red-900/50'
                : 'btn-outline border-red-600 text-red-400 hover:bg-red-600 hover:text-white',
            ]"
          >
            Yoga
          </button>
          <button
            @click="selectedCategory = 'boxing'"
            :class="[
              'btn text-base font-medium px-6 py-3 rounded-full transition-all duration-300',
              selectedCategory === 'boxing'
                ? 'btn-error bg-red-600 text-white shadow-inner shadow-red-900/50'
                : 'btn-outline border-red-600 text-red-400 hover:bg-red-600 hover:text-white',
            ]"
          >
            Boxing
          </button>
          <button
            @click="selectedCategory = 'cardio'"
            :class="[
              'btn text-base font-medium px-6 py-3 rounded-full transition-all duration-300',
              selectedCategory === 'cardio'
                ? 'btn-error bg-red-600 text-white shadow-inner shadow-red-900/50'
                : 'btn-outline border-red-600 text-red-400 hover:bg-red-600 hover:text-white',
            ]"
          >
            Cardio
          </button>
        </div>
      </div>
    </section>

    <!-- Trainers Grid -->
    <section class="py-16">
      <div class="container mx-auto px-6">
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
          <div
            v-for="trainer in filteredTrainers"
            :key="trainer.id"
            class="card bg-gray-900 rounded-xl shadow-lg border border-red-800/30 overflow-hidden transform transition-all duration-300 hover:scale-[1.02] hover:shadow-xl hover:shadow-red-900/50 group"
          >
            <figure class="relative overflow-hidden h-64">
              <img
                :src="trainer.image || 'https://placehold.co/400x300/333333/EFEFEF?text=No+Image'"
                :alt="trainer.name"
                class="w-full h-full object-cover transition-transform duration-500 group-hover:scale-110"
              />
              <div class="absolute top-4 right-4">
                <div class="badge bg-red-600 text-white border-red-600 font-bold text-sm px-3 py-2 rounded-full shadow-md">
                  {{ trainer.category.toUpperCase() }}
                </div>
              </div>
            </figure>

            <div class="card-body p-6 flex flex-col">
              <h3 class="card-title text-white text-2xl font-bold mb-1 group-hover:text-red-400 transition-colors duration-300">{{ trainer.name }}</h3>
              <p class="text-red-500 font-semibold text-lg mb-2">
                {{ trainer.specialization }}
              </p>
              <p class="text-gray-400 text-sm mb-4">{{ trainer.experience }}</p>

              <!-- Rating -->
              <div class="flex items-center gap-2 mb-4">
                <div class="rating rating-sm">
                  <input
                    v-for="star in 5"
                    :key="star"
                    type="radio"
                    :checked="star <= trainer.rating"
                    class="mask mask-star-2 bg-red-500"
                    disabled
                  />
                </div>
                <span class="text-gray-400 text-sm"
                  >({{ trainer.rating }}/5)</span
                >
              </div>

              <!-- Achievements -->
              <div class="mb-4">
                <div class="flex flex-wrap gap-2">
                  <span
                    v-for="achievement in trainer.achievements"
                    :key="achievement"
                    class="badge badge-outline border-red-500 text-red-500 text-xs px-3 py-2 rounded-full"
                  >
                    {{ achievement }}
                  </span>
                </div>
              </div>

              <div class="card-actions justify-between items-center mt-auto pt-4 border-t border-gray-700/50">
                <div class="text-red-500 font-bold text-xl">
                  {{ trainer.price }}
                </div>
                <NuxtLink :to="`/huan-luyen-vien/${trainer.id}`"
                  class="btn bg-red-600 hover:bg-red-700 text-white border-red-600 font-bold text-base px-6 py-3 rounded-full shadow-md transition-all duration-300 hover:scale-105"
                >
                  Xem Chi Tiết <i class="fas fa-arrow-right ml-2"></i>
                </NuxtLink>
              </div>
            </div>
          </div>
        </div>

        <!-- Load More Button -->
        <div class="text-center mt-12" v-if="hasMore">
          <button
            @click="loadMore"
            class="btn btn-outline border-red-600 text-red-400 hover:bg-red-600 hover:text-white text-base font-semibold px-8 py-3 rounded-full shadow-md transition-all duration-300"
          >
            Xem Thêm Huấn Luyện Viên <i class="fas fa-chevron-down ml-2"></i>
          </button>
        </div>
      </div>
    </section>

    <!-- Stats Section (Already designed in previous requests, keeping consistent) -->
    <section class="bg-gray-900 py-16 border-t border-red-700/50 shadow-inner shadow-red-900/20">
      <div class="container mx-auto px-6">
        <div class="grid grid-cols-1 md:grid-cols-4 gap-8">
          <div class="text-center">
            <div class="text-4xl font-bold text-red-500 mb-2">50+</div>
            <div class="text-gray-400">Huấn Luyện Viên</div>
          </div>
          <div class="text-center">
            <div class="text-4xl font-bold text-red-500 mb-2">1000+</div>
            <div class="text-gray-400">Học Viên</div>
          </div>
          <div class="text-center">
            <div class="text-4xl font-bold text-red-500 mb-2">5+</div>
            <div class="text-gray-400">Năm Kinh Nghiệm</div>
          </div>
          <div class="text-center">
            <div class="text-4xl font-bold text-red-500 mb-2">24/7</div>
            <div class="text-gray-400">Hỗ Trợ</div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";

// Meta
useHead({
  title: "Đội Ngũ Huấn Luyện Viên | AlphaGym",
  meta: [
    {
      name: "description",
      content:
        "Gặp gỡ đội ngũ huấn luyện viên chuyên nghiệp với kinh nghiệm nhiều năm trong lĩnh vực fitness, yoga, boxing và cardio tại AlphaGym.",
    },
  ],
});

// Reactive data
const selectedCategory = ref("all");
const displayCount = ref(9);
const hasMore = ref(true);

// Sample trainers data (giữ nguyên)
const trainers = ref([
  {
    id: 1,
    name: "Nguyễn Văn Anh",
    specialization: "Fitness & Bodybuilding",
    category: "fitness",
    experience: "5 năm kinh nghiệm",
    rating: 5,
    price: "500,000 VNĐ/tháng",
    image:
      "https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=400&h=300&fit=crop",
    achievements: ["Certified Personal Trainer", "Nutrition Specialist"],
  },
  {
    id: 2,
    name: "Trần Thị Bình",
    specialization: "Yoga & Meditation",
    category: "yoga",
    experience: "7 năm kinh nghiệm",
    rating: 5,
    price: "400,000 VNĐ/tháng",
    image:
      "https://images.unsplash.com/photo-1544367567-0f2fcb009e0b?w=400&h=300&fit=crop",
    achievements: ["Yoga Alliance Certified", "Meditation Expert"],
  },
  {
    id: 3,
    name: "Lê Hoàng Cường",
    specialization: "Boxing & Kickboxing",
    category: "boxing",
    experience: "6 năm kinh nghiệm",
    rating: 4,
    price: "600,000 VNĐ/tháng",
    image:
      "https://images.unsplash.com/photo-1549476464-37392f717541?w=400&h=300&fit=crop",
    achievements: ["Professional Boxing", "Combat Sports"],
  },
  {
    id: 4,
    name: "Phạm Thị Dương",
    specialization: "Cardio & HIIT",
    category: "cardio",
    experience: "4 năm kinh nghiệm",
    rating: 5,
    price: "450,000 VNĐ/tháng",
    image:
      "https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=400&h=300&fit=crop",
    achievements: ["HIIT Specialist", "Group Fitness"],
  },
  {
    id: 5,
    name: "Võ Minh Hoàng",
    specialization: "Strength Training",
    category: "fitness",
    experience: "8 năm kinh nghiệm",
    rating: 5,
    price: "550,000 VNĐ/tháng",
    image:
      "https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=400&h=300&fit=crop",
    achievements: ["Strength Coach", "Sports Performance"],
  },
  {
    id: 6,
    name: "Đỗ Thị Lan",
    specialization: "Pilates & Stretching",
    category: "yoga",
    experience: "3 năm kinh nghiệm",
    rating: 4,
    price: "350,000 VNĐ/tháng",
    image:
      "https://images.unsplash.com/photo-1544367567-0f2fcb009e0b?w=400&h=300&fit=crop",
    achievements: ["Pilates Certified", "Flexibility Expert"],
  },
  {
    id: 7,
    name: "Bùi Quang Minh",
    specialization: "Muay Thai",
    category: "boxing",
    experience: "5 năm kinh nghiệm",
    rating: 5,
    price: "580,000 VNĐ/tháng",
    image:
      "https://images.unsplash.com/photo-1549476464-37392f717541?w=400&h=300&fit=crop",
    achievements: ["Muay Thai Champion", "Self Defense"],
  },
  {
    id: 8,
    name: "Ngô Thị Ngọc",
    specialization: "Zumba & Dance",
    category: "cardio",
    experience: "4 năm kinh nghiệm",
    rating: 4,
    price: "400,000 VNĐ/tháng",
    image:
      "https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=400&h=300&fit=crop",
    achievements: ["Zumba Certified", "Dance Instructor"],
  },
  {
    id: 9,
    name: "Hoàng Văn Phú",
    specialization: "Powerlifting",
    category: "fitness",
    experience: "6 năm kinh nghiệm",
    rating: 5,
    price: "650,000 VNĐ/tháng",
    image:
      "https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=400&h=300&fit=crop",
    achievements: ["Powerlifting Coach", "Strength Training"],
  },
]);

// Computed properties
const filteredTrainers = computed(() => {
  let filtered = trainers.value;

  if (selectedCategory.value !== "all") {
    filtered = filtered.filter(
      (trainer) => trainer.category === selectedCategory.value
    );
  }

  return filtered.slice(0, displayCount.value);
});

// Methods
const loadMore = () => {
  displayCount.value += 6;
  if (displayCount.value >= trainers.value.length) {
    hasMore.value = false;
  }
};

// Lifecycle
onMounted(() => {
  // Initial setup
  hasMore.value = trainers.value.length > displayCount.value;
});
</script>

<style scoped>
/* Custom text shadow for hero heading */
.custom-hero-text-shadow {
  text-shadow: 0 0 25px rgba(239, 68, 68, 0.9), 0 0 40px rgba(239, 68, 68, 0.7);
}
</style>
