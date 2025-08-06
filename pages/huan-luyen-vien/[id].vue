<template>
  <div class="min-h-screen bg-black text-white py-20 px-6 lg:px-8">
    <div class="container mx-auto">
      <NuxtLink
        to="/huan-luyen-vien"
        class="btn btn-ghost text-red-500 hover:text-red-400 mb-8 text-lg"
      >
        <i class="fas fa-arrow-left mr-2"></i> Quay lại danh sách
      </NuxtLink>

      <!-- Loading State (Removed as we are using mock data) -->
      <!-- Error State (Removed as we are using mock data) -->

      <!-- No Data State -->
      <div v-if="!trainer" class="text-center py-20">
        <div class="text-7xl mb-4 text-red-500">
          <i class="fas fa-exclamation-triangle"></i>
        </div>
        <h3 class="text-3xl font-bold text-gray-200 mb-3">
          Không tìm thấy huấn luyện viên
        </h3>
        <p class="text-gray-400 text-lg">
          Huấn luyện viên này có thể không tồn tại hoặc đã bị xóa.
        </p>
      </div>

      <!-- Trainer Details -->
      <div
        v-else
        class="bg-gray-900 rounded-2xl shadow-2xl border border-red-800/30 overflow-hidden lg:flex"
      >
        <!-- Trainer Image Section -->
        <div class="lg:w-1/3 relative overflow-hidden">
          <img
            :src="getImageUrl(trainer.photo_url)"
            :alt="trainer.member?.full_name || 'Huấn luyện viên'"
            class="w-full h-96 lg:h-full object-cover transition-transform duration-500 hover:scale-105"
            @error="handleImageError"
          />
          <div class="absolute top-6 left-6">
            <div
              class="badge bg-red-600 text-white border-red-600 font-bold text-base px-4 py-2 rounded-full shadow-md"
            >
              {{ trainer.specialty?.toUpperCase() || "CHUNG" }}
            </div>
          </div>
        </div>

        <!-- Trainer Details Section -->
        <div class="lg:w-2/3 p-8 md:p-12 flex flex-col justify-center">
          <h1 class="text-4xl md:text-5xl font-extrabold text-white mb-3">
            {{ trainer.member?.full_name || "Đang cập nhật" }}
          </h1>
          <p class="text-red-500 font-semibold text-2xl mb-4">
            {{ trainer.specialty || "Chuyên môn" }}
          </p>
          <p class="text-gray-300 text-lg mb-6">
            {{
              trainer.experience_years
                ? `${trainer.experience_years} năm kinh nghiệm`
                : "Chưa cập nhật kinh nghiệm"
            }}
          </p>

          <!-- Rating (assuming API provides average_rating) -->
          <div class="flex items-center gap-2 mb-6">
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
            <span class="text-gray-400 text-lg"
              >({{ trainer.average_rating || 0 }}/5)</span
            >
          </div>

          <!-- Certifications -->
          <div class="mb-8">
            <h3 class="text-xl font-bold text-white mb-3">
              Thành tựu & Chứng chỉ:
            </h3>
            <div class="flex flex-wrap gap-3">
              <span
                v-for="certification in getCertificationsArray(
                  trainer.certifications
                )"
                :key="certification"
                class="badge badge-outline border-red-500 text-red-500 text-base px-4 py-2 rounded-full shadow-sm"
              >
                {{ certification }}
              </span>
            </div>
            <p v-if="!trainer.certifications" class="text-gray-400 text-sm">
              Chưa có chứng chỉ nào được liệt kê.
            </p>
          </div>

          <!-- Description/Bio -->
          <div class="mb-8">
            <h3 class="text-xl font-bold text-white mb-3">Giới thiệu:</h3>
            <p class="text-gray-400 leading-relaxed">
              {{
                trainer.bio ||
                "Chưa có thông tin giới thiệu chi tiết cho huấn luyện viên này."
              }}
            </p>
          </div>

          <!-- Price and Contact (assuming API provides price_per_session) -->
          <div
            class="mt-auto pt-6 border-t border-gray-700/50 flex flex-col md:flex-row justify-between items-center gap-6"
          >
            <div class="text-red-500 font-bold text-3xl">
              {{
                trainer.price_per_session
                  ? formatCurrency(trainer.price_per_session) + "/buổi"
                  : "Liên hệ để biết giá"
              }}
            </div>
            <button
              class="btn btn-error text-white font-bold text-lg px-8 py-4 rounded-full shadow-lg hover:bg-red-700 transform transition-transform duration-200 hover:scale-105"
            >
              <i class="fas fa-phone mr-3"></i>Liên hệ ngay
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from "vue";
import { useRoute } from "vue-router";
import { useHead } from "#app";

const route = useRoute();
const trainerId = computed(() => parseInt(route.params.id)); // Lấy ID từ URL và chuyển đổi sang số nguyên

// Mock data for trainers
const mockTrainers = [
  {
    id: 1,
    member: { full_name: "Nguyễn Văn Anh" },
    specialty: "Fitness",
    experience_years: 5,
    average_rating: 5,
    price_per_session: 500000,
    photo_url:
      "https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=400&h=300&fit=crop",
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
    photo_url:
      "https://images.unsplash.com/photo-1544367567-0f2fcb009e0b?w=400&h=300&fit=crop",
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
    photo_url:
      "https://images.unsplash.com/photo-1549476464-37392f717541?w=400&h=300&fit=crop",
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
    photo_url:
      "https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=400&h=300&fit=crop",
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
    photo_url:
      "https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=400&h=300&fit=crop",
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
    photo_url:
      "https://images.unsplash.com/photo-1544367567-0f2fcb009e0b?w=400&h=300&fit=crop",
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
    photo_url:
      "https://images.unsplash.com/photo-1549476464-37392f717541?w=400&h=300&fit=crop",
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
    photo_url:
      "https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=400&h=300&fit=crop",
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
    photo_url:
      "https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=400&h=300&fit=crop",
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
    photo_url:
      "https://images.unsplash.com/photo-1544367567-0f2fcb009e0b?w=400&h=300&fit=crop",
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
    photo_url:
      "https://images.unsplash.com/photo-1549476464-37392f717541?w=400&h=300&fit=crop",
    certifications: "Kickboxing Certified, Self Defense Instructor",
    bio: "Giảng viên Kickboxing, chuyên đào tạo các kỹ thuật phòng thủ và tấn công hiệu quả.",
  },
];

// Find the trainer based on the ID from mock data
const trainer = computed(() => {
  return mockTrainers.find((t) => t.id === trainerId.value);
});

// Helper function to format currency
const formatCurrency = (value) => {
  if (!value || isNaN(value)) return "0 ₫";
  return new Intl.NumberFormat("vi-VN", {
    style: "currency",
    currency: "VND",
    minimumFractionDigits: 0,
    maximumFractionDigits: 0,
  }).format(value);
};

// Helper function to get image URL (now directly returns the URL from mock data)
const getImageUrl = (imageUrl) => {
  if (!imageUrl) {
    return "/placeholder.svg?height=800&width=600"; // Placeholder for vertical images
  }
  return imageUrl;
};

// Handle image loading errors
const handleImageError = (event) => {
  event.target.src = "/placeholder.svg?height=800&width=600";
};

const getCertificationsArray = (certificationsString) => {
  if (!certificationsString) return [];
  return certificationsString
    .split(",")
    .map((s) => s.trim())
    .filter((s) => s !== "");
};

// SEO - Use computed property for dynamic title and description
useHead({
  title: computed(() =>
    trainer.value
      ? `HLV ${trainer.value.member?.full_name} | GymTech`
      : "Chi tiết Huấn luyện viên | GymTech"
  ),
  meta: [
    {
      name: "description",
      content: computed(
        () =>
          trainer.value?.bio ||
          `Thông tin chi tiết về huấn luyện viên ${
            trainer.value?.member?.full_name || ""
          } chuyên về ${trainer.value?.specialty || ""} tại GymTech.`
      ),
    },
  ],
});
</script>

<style scoped>
/* Custom text shadow for heading */
.custom-text-shadow {
  text-shadow: 0 0 20px rgba(239, 68, 68, 0.8), 0 0 30px rgba(239, 68, 68, 0.6);
}
</style>
