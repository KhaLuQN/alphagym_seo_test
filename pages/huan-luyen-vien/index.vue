<template>
  <div class="min-h-screen bg-black text-white">
    <section class="relative py-24 md:py-32 bg-gradient-to-br from-gray-950 via-red-950 to-black overflow-hidden">
      <div class="relative z-10 container mx-auto px-4">
        <div class="text-center">
          <h1 class="text-5xl md:text-7xl font-extrabold mb-6 text-red-500 custom-hero-text-shadow leading-tight">
            <i class="fas fa-users mr-4 text-red-600"></i>
            <span class="text-white">Đội Ngũ</span>
            <span class="text-red-500"> Huấn Luyện Viên</span>
          </h1>
          <p class="text-xl md:text-2xl text-gray-300 mb-10 max-w-xl mx-auto">
            Gặp gỡ đội ngũ huấn luyện viên chuyên nghiệp với kinh nghiệm nhiều
            năm
          </p>
        </div>
      </div>
    </section>

    <section class="py-8 bg-gray-900 border-b border-red-700/50 shadow-inner shadow-red-900/20">
      <div class="container mx-auto px-4">
        <div class="flex flex-wrap gap-4 justify-center">
          <button
            @click="selectedSpecialty = 'all'"
            :class="selectedSpecialty === 'all' ? 'bg-red-700 border-red-700' : ''"
            class="btn bg-red-600 hover:bg-red-700 border-red-600 text-white font-bold text-lg px-6 py-3 rounded-full shadow-lg transform transition-transform duration-200 hover:scale-105"
          >
            Tất cả
          </button>
          <button
            @click="selectedSpecialty = 'Fitness'"
            :class="selectedSpecialty === 'Fitness' ? 'bg-red-700 border-red-700' : ''"
            class="btn bg-red-600 hover:bg-red-700 border-red-600 text-white font-bold text-lg px-6 py-3 rounded-full shadow-lg transform transition-transform duration-200 hover:scale-105"
          >
            Fitness
          </button>
          <button
            @click="selectedSpecialty = 'Yoga'"
            :class="selectedSpecialty === 'Yoga' ? 'bg-red-700 border-red-700' : ''"
            class="btn bg-red-600 hover:bg-red-700 border-red-600 text-white font-bold text-lg px-6 py-3 rounded-full shadow-lg transform transition-transform duration-200 hover:scale-105"
          >
            Yoga
          </button>
          <button
            @click="selectedSpecialty = 'Boxing'"
            :class="selectedSpecialty === 'Boxing' ? 'bg-red-700 border-red-700' : ''"
            class="btn bg-red-600 hover:bg-red-700 border-red-600 text-white font-bold text-lg px-6 py-3 rounded-full shadow-lg transform transition-transform duration-200 hover:scale-105"
          >
            Boxing
          </button>
          <button
            @click="selectedSpecialty = 'Cardio'"
            :class="selectedSpecialty === 'Cardio' ? 'bg-red-700 border-red-700' : ''"
            class="btn bg-red-600 hover:bg-red-700 border-red-600 text-white font-bold text-lg px-6 py-3 rounded-full shadow-lg transform transition-transform duration-200 hover:scale-105"
          >
            Cardio
          </button>
        </div>
      </div>
    </section>

    <section class="py-16">
      <div class="container mx-auto px-4">
        <!-- No Data State -->
        <div v-if="!filteredTrainers || filteredTrainers.length === 0" class="text-center py-12">
          <div class="text-7xl mb-4 text-red-500">
            📦
          </div>
          <h3 class="text-3xl font-bold text-gray-200 mb-3">
            Không tìm thấy huấn luyện viên nào
          </h3>
          <p class="text-gray-400 text-lg">Hiện tại chưa có huấn luyện viên nào được cung cấp hoặc không khớp với bộ lọc.</p>
        </div>

        <!-- Trainers Grid -->
        <div v-else class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
          <div
            v-for="trainer in displayedTrainers"
            :key="trainer.id"
            class="card bg-gray-900 rounded-xl shadow-xl border border-red-800/30 p-8 text-center transform transition-all duration-300 hover:scale-[1.02] hover:shadow-2xl hover:shadow-red-900/50"
          >
            <figure class="relative overflow-hidden">
              <img
                :src="getImageUrl(trainer.photo_url)"
                :alt="trainer.member?.full_name || 'Huấn luyện viên'"
                class="w-full h-64 object-cover transition-transform duration-300 hover:scale-105"
                @error="handleImageError"
              />
              <div class="absolute top-4 right-4">
                <div class="badge bg-red-600 text-white border-red-600">
                  {{ trainer.specialty?.toUpperCase() || 'CHUNG' }}
                </div>
              </div>
            </figure>

            <div class="card-body p-6">
              <h3 class="card-title text-2xl font-extrabold text-white mb-2">{{ trainer.member?.full_name || 'Đang cập nhật' }}</h3>
              <p class="text-red-500 font-semibold text-lg">
                {{ trainer.specialty || 'Chuyên môn' }}
              </p>
              <p class="text-gray-400 text-base mb-4">{{ trainer.experience_years ? `${trainer.experience_years} năm kinh nghiệm` : 'Chưa cập nhật kinh nghiệm' }}</p>

              <div class="flex items-center gap-2 mb-4">
                <div class="rating rating-md">
                  <input
                    v-for="star in 5"
                    :key="star"
                    type="radio"
                    :checked="star <= trainer.average_rating"
                    class="mask mask-star-2 bg-red-500"
                    disabled
                  />
                </div>
                <span class="text-gray-400 text-base"
                  >({{ trainer.average_rating || 0 }}/5)</span
                >
              </div>

              <div class="mb-4">
                <div class="flex flex-wrap gap-2">
                  <span
                    v-for="certification in getCertificationsArray(trainer.certifications)"
                    :key="certification"
                    class="badge badge-outline border-red-500 text-red-500 text-sm px-4 py-2 rounded-full shadow-sm"
                  >
                    {{ certification }}
                  </span>
                </div>
                <p v-if="!trainer.certifications" class="text-gray-400 text-xs">Chưa có chứng chỉ.</p>
              </div>

              <div class="card-actions justify-between items-center">
                <div class="text-red-500 font-bold text-2xl">
                  {{ trainer.price_per_session ? formatCurrency(trainer.price_per_session) : 'Liên hệ để biết giá' }}
                </div>
                <NuxtLink :to="`/huan-luyen-vien/${trainer.id}`" class="btn btn-error text-white font-bold px-6 py-3 rounded-full shadow-lg hover:bg-red-700 transform transition-transform duration-200 hover:scale-105">
                  Xem Chi Tiết
                </NuxtLink>
              </div>
            </div>
          </div>
        </div>

        <div class="text-center mt-12" v-if="hasMore">
          <button
            @click="loadMore"
            class="btn btn-primary bg-red-600 hover:bg-red-700 border-red-600 hover:border-red-700 text-white font-bold text-lg px-8 py-4 rounded-full shadow-lg transform transition-transform duration-200 hover:scale-105 pulse-red"
          >
            Xem Thêm Huấn Luyện Viên
          </button>
        </div>
      </div>
    </section>

    <section class="py-16 bg-gray-900 border-t border-red-700/50 shadow-inner shadow-red-900/20">
      <div class="container mx-auto px-4">
        <div class="grid grid-cols-1 md:grid-cols-4 gap-8">
          <div class="text-center">
            <div class="text-5xl font-extrabold text-red-500 mb-3">50+</div>
            <div class="text-gray-300 text-lg">Huấn Luyện Viên</div>
          </div>
          <div class="text-center">
            <div class="text-5xl font-extrabold text-red-500 mb-3">1000+</div>
            <div class="text-gray-300 text-lg">Học Viên</div>
          </div>
          <div class="text-center">
            <div class="text-5xl font-extrabold text-red-500 mb-3">5+</div>
            <div class="text-gray-300 text-lg">Năm Kinh Nghiệm</div>
          </div>
          <div class="text-center">
            <div class="text-5xl font-extrabold text-red-500 mb-3">24/7</div>
            <div class="text-gray-300 text-lg">Hỗ Trợ</div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup>
import { ref, computed, watch } from "vue";
import { useHead } from '#app';

// Meta
useHead({
  title: "Đội Ngũ Huấn Luyện Viên | Gym & Fitness",
  meta: [
    {
      name: "description",
      content:
        "Gặp gỡ đội ngũ huấn luyện viên chuyên nghiệp với kinh nghiệm nhiều năm trong lĩnh vực fitness, yoga, boxing và cardio tại AlphaGym.",
    },
  ],
});

// Reactive data
const selectedSpecialty = ref("all");
const displayCount = ref(9);
const hasMore = ref(true);

// Mock data for trainers
const allTrainers = ref([
  {
    id: 1,
    member: { full_name: "Nguyễn Văn Anh" },
    specialty: "Fitness",
    experience_years: 5,
    average_rating: 5,
    price_per_session: 500000,
    photo_url: "https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=400&h=300&fit=crop",
    certifications: "Certified Personal Trainer, Nutrition Specialist",
    bio: "Huấn luyện viên chuyên về Fitness và Bodybuilding với nhiều năm kinh nghiệm giúp học viên đạt được mục tiêu thể hình.",
  },
  {
    id: 2,
    member: { full_name: "Trần Thị Bình" },
    specialty: "Yoga",
    experience_years: 7,
    average_rating: 5,
    price_per_session: 400000,
    photo_url: "https://images.unsplash.com/photo-1544367567-0f2fcb009e0b?w=400&h=300&fit=crop",
    certifications: "Yoga Alliance Certified, Meditation Expert",
    bio: "Chuyên gia Yoga và Thiền định, hướng dẫn các lớp học giúp cải thiện sự linh hoạt, sức khỏe tinh thần và thể chất.",
  },
  {
    id: 3,
    member: { full_name: "Lê Hoàng Cường" },
    specialty: "Boxing",
    experience_years: 6,
    average_rating: 4,
    price_per_session: 600000,
    photo_url: "https://images.unsplash.com/photo-1549476464-37392f717541?w=400&h=300&fit=crop",
    certifications: "Professional Boxing Coach, Combat Sports",
    bio: "Huấn luyện viên Boxing và Kickboxing chuyên nghiệp, giúp học viên phát triển kỹ năng tự vệ và sức bền.",
  },
  {
    id: 4,
    member: { full_name: "Phạm Thị Dương" },
    specialty: "Cardio",
    experience_years: 4,
    average_rating: 5,
    price_per_session: 450000,
    photo_url: "https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=400&h=300&fit=crop",
    certifications: "HIIT Specialist, Group Fitness Certified",
    bio: "Đào tạo các bài tập Cardio và HIIT cường độ cao, giúp đốt cháy calo hiệu quả và tăng cường sức khỏe tim mạch.",
  },
  {
    id: 5,
    member: { full_name: "Võ Minh Hoàng" },
    specialty: "Fitness",
    experience_years: 8,
    average_rating: 5,
    price_per_session: 550000,
    photo_url: "https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=400&h=300&fit=crop",
    certifications: "Strength Coach, Sports Performance",
    bio: "Chuyên gia huấn luyện sức mạnh và hiệu suất thể thao, giúp vận động viên và người tập cải thiện sức bền.",
  },
  {
    id: 6,
    member: { full_name: "Đỗ Thị Lan" },
    specialty: "Yoga",
    experience_years: 3,
    average_rating: 4,
    price_per_session: 350000,
    photo_url: "https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=400&h=300&fit=crop",
    certifications: "Pilates Certified, Flexibility Expert",
    bio: "Hướng dẫn Pilates và các bài tập kéo giãn, giúp tăng cường sự dẻo dai và săn chắc cơ thể.",
  },
  {
    id: 7,
    member: { full_name: "Bùi Quang Minh" },
    specialty: "Boxing",
    experience_years: 5,
    average_rating: 5,
    price_per_session: 580000,
    photo_url: "https://images.unsplash.com/photo-1549476464-37392f717541?w=400&h=300&fit=crop",
    certifications: "Muay Thai Champion, Self Defense",
    bio: "Huấn luyện viên Muay Thai chuyên nghiệp, giúp học viên rèn luyện kỹ năng chiến đấu và tự vệ.",
  },
  {
    id: 8,
    member: { full_name: "Ngô Thị Ngọc" },
    specialty: "Cardio",
    experience_years: 4,
    average_rating: 4,
    price_per_session: 400000,
    photo_url: "https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=400&h=300&fit=crop",
    certifications: "Zumba Certified, Dance Instructor",
    bio: "Giáo viên Zumba và Dance, mang đến những buổi tập sôi động, giúp cải thiện thể lực và tâm trạng.",
  },
  {
    id: 9,
    member: { full_name: "Hoàng Văn Phú" },
    specialty: "Fitness",
    experience_years: 6,
    average_rating: 5,
    price_per_session: 650000,
    photo_url: "https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=400&h=300&fit=crop",
    certifications: "Powerlifting Coach, Strength Training",
    bio: "Chuyên gia Powerlifting, giúp học viên đạt được sức mạnh tối đa và kỹ thuật nâng tạ chuẩn xác.",
  },
  {
    id: 10,
    member: { full_name: "Vũ Thị Quỳnh" },
    specialty: "Yoga",
    experience_years: 5,
    average_rating: 4,
    price_per_session: 420000,
    photo_url: "https://images.unsplash.com/photo-1544367567-0f2fcb009e0b?w=400&h=300&fit=crop",
    certifications: "Vinyasa Yoga Instructor, Mindfulness",
    bio: "Huấn luyện viên Vinyasa Yoga, tập trung vào sự kết nối giữa hơi thở và chuyển động.",
  },
  {
    id: 11,
    member: { full_name: "Trần Duy Khoa" },
    specialty: "Boxing",
    experience_years: 7,
    average_rating: 5,
    price_per_session: 620000,
    photo_url: "https://images.unsplash.com/photo-1549476464-37392f717541?w=400&h=300&fit=crop",
    certifications: "Kickboxing Certified, Self Defense Instructor",
    bio: "Giảng viên Kickboxing, chuyên đào tạo các kỹ thuật phòng thủ và tấn công hiệu quả.",
  },
]);

// Computed properties
const filteredTrainers = computed(() => {
  let filtered = allTrainers.value;

  if (selectedSpecialty.value !== "all") {
    filtered = filtered.filter(
      (trainer) => trainer.specialty?.toLowerCase() === selectedSpecialty.value.toLowerCase()
    );
  }
  return filtered;
});

const displayedTrainers = computed(() => {
  return filteredTrainers.value.slice(0, displayCount.value);
});

// Watch for changes in filteredTrainers or displayCount to update hasMore
watch([filteredTrainers, displayCount], () => {
  hasMore.value = filteredTrainers.value.length > displayCount.value;
}, { immediate: true });

// Methods
const loadMore = () => {
  displayCount.value += 6;
};

// Helper function to format currency
const formatCurrency = (value) => {
  if (!value || isNaN(value)) return '0 ₫';
  return new Intl.NumberFormat('vi-VN', {
    style: 'currency',
    currency: 'VND',
    minimumFractionDigits: 0,
    maximumFractionDigits: 0,
  }).format(value);
};

// Helper function to get image URL (now directly returns the URL from mock data)
const getImageUrl = (imageUrl) => {
  if (!imageUrl) {
    return '/placeholder.svg?height=800&width=600'; // Placeholder for vertical images
  }
  return imageUrl;
};

// Handle image loading errors
const handleImageError = (event) => {
  event.target.src = '/placeholder.svg?height=800&width=600';
};

const getCertificationsArray = (certificationsString) => {
  if (!certificationsString) return [];
  return certificationsString.split(',').map(s => s.trim()).filter(s => s !== '');
};
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

.custom-hero-text-shadow {
  text-shadow: 0 0 25px rgba(239, 68, 68, 0.9), 0 0 40px rgba(239, 68, 68, 0.7);
}

.pulse-red {
  animation: pulse-red-effect 2s infinite;
}

@keyframes pulse-red-effect {
  0% {
    box-shadow: 0 0 0 0 rgba(239, 68, 68, 0.7);
  }
  70% {
    box-shadow: 0 0 0 15px rgba(239, 68, 68, 0);
  }
  100% {
    box-shadow: 0 0 0 0 rgba(239, 68, 68, 0);
  }
}

.custom-text-shadow {
  text-shadow: 0 0 20px rgba(239, 68, 68, 0.8), 0 0 30px rgba(239, 68, 68, 0.6);
}
</style>
