<template>
  <div class="min-h-screen bg-black text-white">
    <!-- Hero Section -->
    <section class="hero-gradient py-16 md:py-24 relative overflow-hidden">
      <div class="absolute inset-0 bg-black/20"></div>
      <div class="relative z-10 container mx-auto px-4 text-center">
        <div class="mb-6">
          <i class="fas fa-dumbbell text-red-500 text-6xl mb-4"></i>
        </div>
        <h1
          class="text-5xl md:text-7xl font-extrabold mb-6 text-transparent bg-clip-text bg-gradient-to-r from-white to-red-500"
        >
          AI Tư vấn Fitness
        </h1>
        <p class="text-xl md:text-2xl text-gray-300 mb-8 max-w-2xl mx-auto">
          Công nghệ AI thông minh giúp bạn đạt mục tiêu thể hình nhanh chóng và
          hiệu quả
        </p>
        <div class="flex justify-center gap-8 text-sm text-gray-400">
          <div class="flex items-center gap-2">
            <i class="fas fa-check-circle text-green-500"></i>
            <span>Tư vấn cá nhân hóa</span>
          </div>
          <div class="flex items-center gap-2">
            <i class="fas fa-chart-line text-blue-500"></i>
            <span>Kế hoạch khoa học</span>
          </div>
          <div class="flex items-center gap-2">
            <i class="fas fa-users text-purple-500"></i>
            <span>Hỗ trợ 24/7</span>
          </div>
        </div>
      </div>
    </section>

    <!-- Form Section -->
    <section class="py-12 px-4">
      <div class="container mx-auto max-w-4xl">
        <div class="card bg-base-100 shadow-2xl">
          <div class="card-body">
            <h2 class="card-title text-3xl justify-center mb-8 text-primary">
              <i class="fas fa-clipboard-list mr-3"></i>
              Thông tin của bạn
            </h2>

            <form id="consultForm" class="grid md:grid-cols-2 gap-6">
              <div class="form-control">
                <label class="label">
                  <span class="label-text font-semibold">
                    <i class="fas fa-user mr-2 text-primary"></i>Họ và tên
                  </span>
                </label>
                <input
                  name="name"
                  type="text"
                  placeholder="Nhập họ tên của bạn"
                  class="input input-bordered input-primary"
                  required
                />
              </div>

              <div class="form-control">
                <label class="label">
                  <span class="label-text font-semibold">
                    <i class="fas fa-venus-mars mr-2 text-primary"></i>Giới tính
                  </span>
                </label>
                <select
                  name="gender"
                  class="select select-bordered select-primary"
                  required
                >
                  <option value="" disabled selected>Chọn giới tính</option>
                  <option value="Nam">Nam</option>
                  <option value="Nữ">Nữ</option>
                </select>
              </div>

              <div class="form-control">
                <label class="label">
                  <span class="label-text font-semibold">
                    <i class="fas fa-birthday-cake mr-2 text-primary"></i>Tuổi
                  </span>
                </label>
                <input
                  name="age"
                  type="number"
                  placeholder="Nhập tuổi của bạn"
                  class="input input-bordered input-primary"
                  min="1"
                  max="100"
                  required
                />
              </div>

              <div class="form-control">
                <label class="label">
                  <span class="label-text font-semibold">
                    <i class="fas fa-ruler-vertical mr-2 text-primary"></i>Chiều
                    cao (cm)
                  </span>
                </label>
                <input
                  name="height"
                  type="number"
                  placeholder="Ví dụ: 170"
                  class="input input-bordered input-primary"
                  min="100"
                  max="250"
                  required
                />
              </div>

              <div class="form-control">
                <label class="label">
                  <span class="label-text font-semibold">
                    <i class="fas fa-weight mr-2 text-primary"></i>Cân nặng (kg)
                  </span>
                </label>
                <input
                  name="weight"
                  type="number"
                  placeholder="Ví dụ: 65"
                  class="input input-bordered input-primary"
                  min="30"
                  max="200"
                  required
                />
              </div>

              <div class="form-control">
                <label class="label">
                  <span class="label-text font-semibold">
                    <i class="fas fa-calendar-week mr-2 text-primary"></i>Số
                    buổi tập/tuần
                  </span>
                </label>
                <input
                  name="weekly_sessions"
                  type="number"
                  placeholder="Ví dụ: 3"
                  class="input input-bordered input-primary"
                  min="1"
                  max="7"
                  required
                />
              </div>

              <div class="form-control md:col-span-2">
                <label class="label">
                  <span class="label-text font-semibold">
                    <i class="fas fa-target mr-2 text-primary"></i>Mục tiêu tập
                    luyện
                  </span>
                </label>
                <input
                  name="goal"
                  type="text"
                  placeholder="Ví dụ: Giảm mỡ bụng, tăng cơ toàn thân, cải thiện sức khỏe..."
                  class="input input-bordered input-primary"
                  required
                />
              </div>

              <div class="form-control">
                <label class="label">
                  <span class="label-text font-semibold">
                    <i class="fas fa-clock mr-2 text-primary"></i>Thời gian mong
                    muốn
                  </span>
                </label>
                <input
                  name="goal_time"
                  type="text"
                  placeholder="Ví dụ: 3 tháng, 6 tháng..."
                  class="input input-bordered input-primary"
                  required
                />
              </div>

              <div class="form-control">
                <label class="label">
                  <span class="label-text font-semibold">
                    <i class="fas fa-activity mr-2 text-primary"></i>Mức độ vận
                    động
                  </span>
                </label>
                <select
                  name="activity_level"
                  class="select select-bordered select-primary"
                  required
                >
                  <option value="" disabled selected>
                    Chọn mức độ vận động
                  </option>
                  <option value="Thấp">Thấp - Ít vận động</option>
                  <option value="Trung bình">
                    Trung bình - Vận động đều đặn
                  </option>
                  <option value="Cao">Cao - Vận động nhiều</option>
                </select>
              </div>

              <div class="md:col-span-2 mt-6">
                <button
                  type="submit"
                  class="btn btn-primary btn-lg w-full text-white font-bold"
                >
                  <span id="buttonText">
                    <i class="fas fa-brain mr-2"></i>
                    Nhận tư vấn từ AI
                  </span>
                  <span id="loadingSpinner" class="hidden">
                    <div class="loading-spinner mr-2"></div>
                    AI đang phân tích...
                  </span>
                </button>
              </div>
            </form>
          </div>
        </div>

        <!-- Results Section -->
        <div id="resultsSection" class="hidden mt-8 fade-in">
          <div class="card bg-base-100 shadow-2xl">
            <div class="card-body">
              <h2 class="card-title text-3xl justify-center mb-8 text-success">
                <i class="fas fa-check-circle mr-3"></i>
                Kết quả tư vấn
              </h2>
              <div id="resultsContent"></div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
document
  .getElementById("consultForm")
  .addEventListener("submit", async function (e) {
    e.preventDefault();

    // Show loading state
    const buttonText = document.getElementById("buttonText");
    const loadingSpinner = document.getElementById("loadingSpinner");
    const submitButton = e.target.querySelector('button[type="submit"]');

    buttonText.classList.add("hidden");
    loadingSpinner.classList.remove("hidden");
    submitButton.disabled = true;

    // Hide previous results
    document.getElementById("resultsSection").classList.add("hidden");

    // Collect form data
    const formData = new FormData(e.target);
    const data = Object.fromEntries(formData.entries());

    try {
      // Simulate API call (replace with actual API endpoint)
      const response = await fetch("/api/consult", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(data),
      });

      const result = await response.json();
      displayResults(result.message);
    } catch (error) {
      console.error("Error:", error);
      // Display mock result for demo
      const mockResult = `**📊 1. ĐÁNH GIÁ THỂ TRẠNG HIỆN TẠI:**
BMI của bạn: 22.5 (Bình thường)
Thể trạng hiện tại ở mức trung bình, phù hợp để bắt đầu chương trình tập luyện.

**🎯 2. PHÂN TÍCH MỤC TIÊU:**
Với 3 buổi tập/tuần, bạn có thể đạt mục tiêu giảm 5kg trong 3-4 tháng.
Mục tiêu này hoàn toàn khả thi với kế hoạch phù hợp.

**💪 3. LỊCH TẬP ĐỀ XUẤT:**
- Thứ 2: Cardio + Bài tập toàn thân
- Thứ 4: Tập tạ + Yoga
- Thứ 6: HIIT + Stretching

**🥗 4. CHỂ ĐỘ DINH DƯỠNG:**
- Ăn nhiều: Protein, rau xanh, trái cây
- Hạn chế: Đồ chiên rán, đồ ngọt
- Uống đủ 2-3 lít nước/ngày

**🌟 5. LỜI KHUYÊN:**
Kiên trì là chìa khóa thành công! Hãy bắt đầu từ từ và tăng cường độ dần dần.

**🎁 GỢI Ý GÓI TẬP:**
Gói Slim Fit - 3 buổi/tuần, phù hợp với mục tiêu giảm cân của bạn.

**👨‍🏫 HUẤN LUYỆN VIÊN ĐỀ XUẤT:**
PT Linh - Chuyên về giảm cân, hỗ trợ tâm lý tốt, phù hợp với người mới bắt đầu.`;

      displayResults(mockResult);
    } finally {
      // Reset button state
      buttonText.classList.remove("hidden");
      loadingSpinner.classList.add("hidden");
      submitButton.disabled = false;
    }
  });

function displayResults(resultText) {
  const resultsContent = document.getElementById("resultsContent");
  const resultsSection = document.getElementById("resultsSection");

  // Parse and format the result
  const sections = parseResultSections(resultText);

  resultsContent.innerHTML = sections
    .map(
      (section, index) => `
                <div class="result-card rounded-lg p-6 mb-6">
                    <div class="flex items-start gap-4">
                        <div class="section-icon w-12 h-12 rounded-full flex items-center justify-center flex-shrink-0">
                            <i class="fas ${getSectionIcon(
                              index
                            )} text-white text-lg"></i>
                        </div>
                        <div class="flex-1">
                            <h3 class="text-xl font-bold mb-3 text-white">${
                              section.title
                            }</h3>
                            <div class="text-gray-300 leading-relaxed">${
                              section.content
                            }</div>
                        </div>
                    </div>
                </div>
            `
    )
    .join("");

  resultsSection.classList.remove("hidden");
  resultsSection.scrollIntoView({ behavior: "smooth" });
}

function parseResultSections(text) {
  const sections = [];
  const lines = text.split("\n");
  let currentSection = null;

  for (const line of lines) {
    if (line.includes("**") && line.includes(":")) {
      if (currentSection) {
        sections.push(currentSection);
      }
      currentSection = {
        title: line.replace(/\*\*/g, "").trim(),
        content: "",
      };
    } else if (currentSection && line.trim()) {
      currentSection.content += line + "<br>";
    }
  }

  if (currentSection) {
    sections.push(currentSection);
  }

  return sections;
}

function getSectionIcon(index) {
  const icons = [
    "fa-heartbeat", // Đánh giá thể trạng
    "fa-bullseye", // Phân tích mục tiêu
    "fa-calendar-alt", // Lịch tập
    "fa-apple-alt", // Dinh dưỡng
    "fa-star", // Lời khuyên
    "fa-gift", // Gói tập
  ];
  return icons[index] || "fa-info-circle";
}
</script>
<style scoped>
.hero-gradient {
  background: linear-gradient(
    135deg,
    #0f0f23 0%,
    #1a0a2e 25%,
    #16213e 50%,
    #0f3460 75%,
    #533a71 100%
  );
}
.loading-spinner {
  border: 3px solid #f3f3f3;
  border-top: 3px solid #ef4444;
  border-radius: 50%;
  width: 24px;
  height: 24px;
  animation: spin 1s linear infinite;
}
@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
.fade-in {
  animation: fadeIn 0.8s ease-in-out;
}
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
.result-card {
  background: linear-gradient(135deg, #1e293b 0%, #334155 100%);
  border: 1px solid #475569;
}
.section-icon {
  background: linear-gradient(135deg, #ef4444 0%, #dc2626 100%);
}
</style>
