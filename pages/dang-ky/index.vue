<template>
  <div class="min-h-screen bg-black text-white">
    <!-- Hero Section -->
    <div
      class="relative py-24 md:py-32 bg-gradient-to-br from-gray-950 via-red-950 to-black overflow-hidden"
    >
      <div class="container mx-auto px-6 relative z-10">
        <div class="max-w-4xl mx-auto text-center">
          <h1 class="text-5xl md:text-7xl font-extrabold mb-6 text-red-500 custom-hero-text-shadow leading-tight">
            <i class="fas fa-clipboard-list mr-4 text-red-600"></i>Đăng Ký Thông Tin
          </h1>
          <p class="text-xl md:text-2xl text-gray-300 mb-10 max-w-xl mx-auto">
            Vui lòng điền thông tin của bạn để chúng tôi liên hệ!
          </p>
        </div>
      </div>
    </div>

    <!-- Registration Form Section -->
    <section class="py-16 bg-gray-900 border-b border-red-700/50 shadow-inner shadow-red-900/20">
      <div class="container mx-auto px-6 lg:px-8 max-w-2xl">
        <div class="card bg-gray-900 rounded-2xl shadow-2xl border border-red-800/30 p-8 md:p-12">
          <h2 class="text-3xl md:text-4xl font-extrabold text-red-400 text-center mb-8 custom-text-shadow">
            Gửi thông tin của bạn
          </h2>
          <form @submit.prevent="submitForm" class="space-y-6">
            <div class="form-control">
              <label class="label">
                <span class="label-text text-gray-300">Họ và tên</span>
              </label>
              <input
                type="text"
                v-model="fullName"
                placeholder="Nhập họ và tên của bạn"
                class="input input-bordered input-lg w-full bg-gray-800 text-white placeholder-gray-400 border-red-700 focus:border-red-500 focus:ring-2 focus:ring-red-500/50 transition-all duration-300"
                required
              />
            </div>

            <div class="form-control">
              <label class="label">
                <span class="label-text text-gray-300">Email</span>
              </label>
              <input
                type="email"
                v-model="email"
                placeholder="Nhập địa chỉ email của bạn"
                class="input input-bordered input-lg w-full bg-gray-800 text-white placeholder-gray-400 border-red-700 focus:border-red-500 focus:ring-2 focus:ring-red-500/50 transition-all duration-300"
                required
              />
            </div>

            <div class="form-control">
              <label class="label">
                <span class="label-text text-gray-300">Số điện thoại</span>
              </label>
              <input
                type="tel"
                v-model="phone"
                placeholder="Nhập số điện thoại của bạn"
                class="input input-bordered input-lg w-full bg-gray-800 text-white placeholder-gray-400 border-red-700 focus:border-red-500 focus:ring-2 focus:ring-red-500/50 transition-all duration-300"
                required
              />
            </div>

            <div class="form-control">
              <label class="label">
                <span class="label-text text-gray-300">Năm sinh</span>
              </label>
              <input
                type="number"
                v-model="birthYear"
                placeholder="Năm sinh của bạn (VD: 1990)"
                class="input input-bordered input-lg w-full bg-gray-800 text-white placeholder-gray-400 border-red-700 focus:border-red-500 focus:ring-2 focus:ring-red-500/50 transition-all duration-300"
                min="1900" :max="new Date().getFullYear()"
                required
              />
            </div>

            <button type="submit" class="btn btn-error btn-block font-bold text-white text-lg py-3 rounded-full shadow-lg hover:bg-red-700 transform transition-transform duration-200 hover:scale-105" :disabled="isSubmitting">
              <span v-if="isSubmitting" class="loading loading-spinner loading-sm"></span>
              <i v-else class="fas fa-paper-plane mr-2"></i>
              {{ isSubmitting ? 'Đang gửi...' : 'GỬI THÔNG TIN' }}
            </button>
          </form>
        </div>
      </div>
    </section>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const fullName = ref('');
const email = ref('');
const phone = ref('');
const birthYear = ref('');
const isSubmitting = ref(false);

const submitForm = async () => {
  isSubmitting.value = true;
  console.log("Form thông tin đang được gửi...");

  // Prepare data for the prompt
  const registrationDetails = `
    Họ và tên: ${fullName.value}
    Email: ${email.value}
    Số điện thoại: ${phone.value}
    Năm sinh: ${birthYear.value}
  `;

  // Prompt for Gemini API to simulate email content
  const prompt = `Tạo một nội dung email ngắn gọn, thân thiện để xác nhận việc nhận thông tin đăng ký của người dùng. Nội dung email nên bao gồm các thông tin sau:
  ${registrationDetails}
  Email này sẽ được gửi đến người dùng để xác nhận thông tin của họ. Bắt đầu bằng "Chào [Tên người dùng]," và kết thúc bằng lời cảm ơn từ "Đội ngũ AlphaGym".`;

  try {
    let chatHistory = [];
    chatHistory.push({ role: "user", parts: [{ text: prompt }] });
    const payload = { contents: chatHistory };
    const apiKey = ""; // If you want to use models other than gemini-2.0-flash or imagen-3.0-generate-002, provide an API key here. Otherwise, leave this as-is.
    const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

    const response = await fetch(apiUrl, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(payload)
    });

    const result = await response.json();

    if (result.candidates && result.candidates.length > 0 &&
        result.candidates[0].content && result.candidates[0].content.parts &&
        result.candidates[0].content.parts.length > 0) {
      const emailContent = result.candidates[0].content.parts[0].text;
      console.log("Nội dung email đã được tạo (mô phỏng gửi):", emailContent);
      console.log("Thông tin đã được gửi thành công! (Mô phỏng)");
      alert("Thông tin của bạn đã được gửi thành công! Chúng tôi sẽ liên hệ lại sớm."); // Using alert for immediate feedback
    } else {
      console.error("Lỗi: Không thể tạo nội dung email từ API Gemini.", result);
      alert("Gửi thông tin thất bại: Không thể tạo email xác nhận. Vui lòng thử lại.");
    }

  } catch (error) {
    console.error("Lỗi khi gửi form hoặc gọi API Gemini:", error);
    alert("Gửi thông tin thất bại: Đã xảy ra lỗi. Vui lòng thử lại.");
  } finally {
    isSubmitting.value = false;
    // Reset form (optional)
    fullName.value = '';
    email.value = '';
    phone.value = '';
    birthYear.value = '';
  }
};

// SEO
useHead({
  title: "Đăng Ký Thông Tin - AlphaGym",
  meta: [
    {
      name: "description",
      content:
        "Điền thông tin của bạn để AlphaGym có thể liên hệ và tư vấn về các gói tập, huấn luyện viên và chương trình sức khỏe phù hợp.",
    },
    {
      name: "keywords",
      content:
        "đăng ký thông tin, liên hệ gym, AlphaGym, tư vấn gym, fitness, sức khỏe",
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
    box-shadow: 0 0 0 0 rgba(239, 68, 68, 0.7);
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
</style>
