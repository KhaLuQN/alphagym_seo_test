<template>
  <div class="min-h-screen bg-black text-white py-20 px-6 lg:px-8">
    <div class="container mx-auto">
      <NuxtLink to="/huan-luyen-vien" class="btn btn-ghost text-red-500 hover:text-red-400 mb-8 text-lg">
        <i class="fas fa-arrow-left mr-2"></i> Quay lại danh sách
      </NuxtLink>

      <div v-if="trainer" class="bg-gray-900 rounded-2xl shadow-2xl border border-red-800/30 overflow-hidden lg:flex">
        <!-- Trainer Image Section -->
        <div class="lg:w-1/3 relative overflow-hidden">
          <img
            :src="trainer.image || 'https://placehold.co/600x800/333333/EFEFEF?text=No+Image'"
            :alt="trainer.name"
            class="w-full h-96 lg:h-full object-cover transition-transform duration-500 hover:scale-105"
          />
          <div class="absolute top-6 left-6">
            <div class="badge bg-red-600 text-white border-red-600 font-bold text-base px-4 py-2 rounded-full shadow-md">
              {{ trainer.category.toUpperCase() }}
            </div>
          </div>
        </div>

        <!-- Trainer Details Section -->
        <div class="lg:w-2/3 p-8 md:p-12 flex flex-col justify-center">
          <h1 class="text-4xl md:text-5xl font-extrabold text-white mb-3">{{ trainer.name }}</h1>
          <p class="text-red-500 font-semibold text-2xl mb-4">{{ trainer.specialization }}</p>
          <p class="text-gray-300 text-lg mb-6">{{ trainer.experience }}</p>

          <!-- Rating -->
          <div class="flex items-center gap-2 mb-6">
            <div class="rating rating-md">
              <input
                v-for="star in 5"
                :key="star"
                type="radio"
                :checked="star <= trainer.rating"
                class="mask mask-star-2 bg-red-500"
                disabled
              />
            </div>
            <span class="text-gray-400 text-lg"
              >({{ trainer.rating }}/5)</span
            >
          </div>

          <!-- Achievements -->
          <div class="mb-8">
            <h3 class="text-xl font-bold text-white mb-3">Thành tựu & Chứng chỉ:</h3>
            <div class="flex flex-wrap gap-3">
              <span
                v-for="achievement in trainer.achievements"
                :key="achievement"
                class="badge badge-outline border-red-500 text-red-500 text-base px-4 py-2 rounded-full shadow-sm"
              >
                {{ achievement }}
              </span>
            </div>
          </div>

          <!-- Description/Bio -->
          <div class="mb-8">
            <h3 class="text-xl font-bold text-white mb-3">Giới thiệu:</h3>
            <p class="text-gray-400 leading-relaxed">
              {{ trainer.bio || 'Chưa có thông tin giới thiệu chi tiết cho huấn luyện viên này.' }}
            </p>
          </div>

          <!-- Price and Contact -->
          <div class="mt-auto pt-6 border-t border-gray-700/50 flex flex-col md:flex-row justify-between items-center gap-6">
            <div class="text-red-500 font-bold text-3xl">
              {{ trainer.price }}
            </div>
            <button
              class="btn btn-error text-white font-bold text-lg px-8 py-4 rounded-full shadow-lg hover:bg-red-700 transform transition-transform duration-200 hover:scale-105"
            >
              <i class="fas fa-phone mr-3"></i>Liên hệ ngay
            </button>
          </div>
        </div>
      </div>

      <div v-else class="text-center py-20">
        <div class="text-7xl mb-4 text-red-500">
          <i class="fas fa-exclamation-triangle"></i>
        </div>
        <h3 class="text-3xl font-bold text-gray-200 mb-3">
          Không tìm thấy huấn luyện viên
        </h3>
        <p class="text-gray-400 text-lg">Huấn luyện viên này có thể không tồn tại hoặc đã bị xóa.</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";
import { useRoute } from "vue-router"; // Import useRoute

const route = useRoute(); // Get the current route
const trainer = ref(null); // Use ref to hold the trainer data

// Sample trainers data (giữ nguyên từ index.vue để mô phỏng)
const trainersData = [
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
    bio: "Nguyễn Văn Anh là một huấn luyện viên chuyên nghiệp với 5 năm kinh nghiệm trong lĩnh vực fitness và bodybuilding. Anh ấy đã giúp hàng trăm học viên đạt được mục tiêu về vóc dáng và sức khỏe. Với kiến thức sâu rộng về dinh dưỡng và tập luyện, Anh Anh cam kết mang lại hiệu quả tối ưu cho từng cá nhân."
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
    bio: "Trần Thị Bình là một chuyên gia Yoga và Thiền định với 7 năm kinh nghiệm. Cô ấy tập trung vào việc giúp học viên tìm thấy sự cân bằng giữa thể chất và tinh thần, cải thiện sự linh hoạt và giảm căng thẳng thông qua các bài tập yoga và kỹ thuật thiền định sâu sắc."
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
    bio: "Lê Hoàng Cường là một huấn luyện viên Boxing và Kickboxing với 6 năm kinh nghiệm. Anh ấy chuyên về các bài tập cường độ cao, giúp học viên phát triển sức mạnh, tốc độ và kỹ năng tự vệ. Các buổi tập của anh Cường luôn đầy năng lượng và thử thách."
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
    bio: "Phạm Thị Dương là huấn luyện viên chuyên về Cardio và HIIT, với 4 năm kinh nghiệm trong việc thiết kế các chương trình tập luyện đốt cháy calo hiệu quả. Cô ấy giúp học viên cải thiện sức bền, tăng cường trao đổi chất và đạt được vóc dáng săn chắc."
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
    bio: "Võ Minh Hoàng là một huấn luyện viên kỳ cựu với 8 năm kinh nghiệm trong lĩnh vực Strength Training. Anh ấy chuyên về việc xây dựng sức mạnh toàn diện, tối ưu hóa hiệu suất thể thao và phòng ngừa chấn thương cho học viên ở mọi cấp độ."
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
    bio: "Đỗ Thị Lan là huấn luyện viên Pilates và Stretching, giúp học viên cải thiện sự dẻo dai, tăng cường cơ lõi và phục hồi cơ thể. Cô ấy tạo ra các buổi tập thư giãn nhưng hiệu quả, phù hợp cho mọi lứa tuổi và trình độ."
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
    bio: "Bùi Quang Minh là một võ sĩ và huấn luyện viên Muay Thai đầy nhiệt huyết. Với 5 năm kinh nghiệm, anh ấy không chỉ dạy kỹ thuật mà còn truyền cảm hứng cho học viên về tinh thần chiến đấu và kỷ luật. Các lớp học của anh Minh giúp bạn rèn luyện cả thể chất lẫn ý chí."
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
    bio: "Ngô Thị Ngọc là huấn luyện viên Zumba và Dance, mang đến những buổi tập tràn đầy năng lượng và niềm vui. Cô ấy giúp học viên đốt cháy calo, cải thiện vóc dáng và giải tỏa căng thẳng thông qua các điệu nhảy sôi động và âm nhạc lôi cuốn."
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
    bio: "Hoàng Văn Phú là chuyên gia Powerlifting với 6 năm kinh nghiệm, chuyên về các bài tập sức mạnh tối đa. Anh ấy hướng dẫn học viên cách nâng tạ an toàn và hiệu quả, giúp họ phá vỡ giới hạn bản thân và đạt được những kỷ lục mới trong powerlifting."
  },
];


// Fetch trainer data based on ID
onMounted(() => {
  const trainerId = parseInt(route.params.id);
  trainer.value = trainersData.find(t => t.id === trainerId);
});

// SEO (giữ nguyên)
useHead({
  title: computed(() => trainer.value ? `HLV ${trainer.value.name} | AlphaGym` : 'Chi tiết Huấn luyện viên | AlphaGym'),
  meta: [
    {
      name: "description",
      content: computed(() => trainer.value ? `Thông tin chi tiết về huấn luyện viên ${trainer.value.name} chuyên về ${trainer.value.specialization} tại AlphaGym.` : 'Thông tin chi tiết về huấn luyện viên tại AlphaGym.'),
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
