<template>
  <div class="min-h-screen bg-black text-white">
    <!-- Hero Section -->
    <section class="bg-gradient-to-r from-black via-red-900 to-black py-16">
      <div class="container mx-auto px-4">
        <div class="text-center">
          <h1 class="text-4xl md:text-6xl font-bold mb-4">
            <span class="text-white">Đội Ngũ</span>
            <span class="text-red-500"> Huấn Luyện Viên</span>
          </h1>
          <p class="text-xl text-gray-300 max-w-2xl mx-auto">
            Gặp gỡ đội ngũ huấn luyện viên chuyên nghiệp với kinh nghiệm nhiều
            năm
          </p>
        </div>
      </div>
    </section>

    <!-- Filters Section -->
    <section class="bg-gray-900 py-8">
      <div class="container mx-auto px-4">
        <div class="flex flex-wrap gap-4 justify-center">
          <button
            @click="selectedCategory = 'all'"
            :class="selectedCategory === 'all' ? 'btn-active' : ''"
            class="btn bg-red-600 hover:bg-red-700 border-red-600 text-white"
          >
            Tất cả
          </button>
          <button
            @click="selectedCategory = 'fitness'"
            :class="selectedCategory === 'fitness' ? 'btn-active' : ''"
            class="btn bg-red-600 hover:bg-red-700 border-red-600 text-white"
          >
            Fitness
          </button>
          <button
            @click="selectedCategory = 'yoga'"
            :class="selectedCategory === 'yoga' ? 'btn-active' : ''"
            class="btn bg-red-600 hover:bg-red-700 border-red-600 text-white"
          >
            Yoga
          </button>
          <button
            @click="selectedCategory = 'boxing'"
            :class="selectedCategory === 'boxing' ? 'btn-active' : ''"
            class="btn bg-red-600 hover:bg-red-700 border-red-600 text-white"
          >
            Boxing
          </button>
          <button
            @click="selectedCategory = 'cardio'"
            :class="selectedCategory === 'cardio' ? 'btn-active' : ''"
            class="btn bg-red-600 hover:bg-red-700 border-red-600 text-white"
          >
            Cardio
          </button>
        </div>
      </div>
    </section>

    <!-- Trainers Grid -->
    <section class="py-16">
      <div class="container mx-auto px-4">
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
          <div
            v-for="trainer in filteredTrainers"
            :key="trainer.id"
            class="card bg-gray-900 border border-gray-800 hover:border-red-500 transition-all duration-300 hover:shadow-lg hover:shadow-red-500/20"
          >
            <figure class="relative overflow-hidden">
              <img
                :src="trainer.image"
                :alt="trainer.name"
                class="w-full h-64 object-cover transition-transform duration-300 hover:scale-105"
              />
              <div class="absolute top-4 right-4">
                <div class="badge bg-red-600 text-white border-red-600">
                  {{ trainer.category }}
                </div>
              </div>
            </figure>

            <div class="card-body">
              <h3 class="card-title text-white text-xl">{{ trainer.name }}</h3>
              <p class="text-red-500 font-semibold">
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
                    class="badge badge-outline border-red-500 text-red-500 text-xs"
                  >
                    {{ achievement }}
                  </span>
                </div>
              </div>

              <div class="card-actions justify-between items-center">
                <div class="text-red-500 font-bold text-lg">
                  {{ trainer.price }}
                </div>
                <button
                  class="btn bg-red-600 hover:bg-red-700 text-white border-red-600"
                >
                  Xem Chi Tiết
                </button>
              </div>
            </div>
          </div>
        </div>

        <!-- Load More Button -->
        <div class="text-center mt-12" v-if="hasMore">
          <button
            @click="loadMore"
            class="btn btn-outline border-red-500 text-red-500 hover:bg-red-500 hover:text-white"
          >
            Xem Thêm Huấn Luyện Viên
          </button>
        </div>
      </div>
    </section>

    <!-- Stats Section -->
    <section class="bg-gray-900 py-16">
      <div class="container mx-auto px-4">
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
  title: "Đội Ngũ Huấn Luyện Viên | Gym & Fitness",
  meta: [
    {
      name: "description",
      content:
        "Gặp gỡ đội ngũ huấn luyện viên chuyên nghiệp với kinh nghiệm nhiều năm trong lĩnh vực fitness, yoga, boxing và cardio.",
    },
  ],
});

// Reactive data
const selectedCategory = ref("all");
const displayCount = ref(9);
const hasMore = ref(true);

// Sample trainers data
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
.btn-active {
  @apply bg-red-700 border-red-700;
}

.rating input:checked {
  @apply bg-red-500;
}

.card:hover {
  transform: translateY(-5px);
}

.badge {
  @apply text-xs font-medium;
}
</style>
