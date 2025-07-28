<template>
  <div class="min-h-screen flex flex-col items-center justify-center">
    <div
      class="animate-spin rounded-full h-16 w-16 border-t-4 border-red-500 mb-4"
    ></div>
    <p>Đang xử lý kết quả thanh toán...</p>
  </div>
</template>

<script setup>
import { useRoute, useRouter } from "vue-router";
import { onMounted } from "vue";
import { useToast } from "vue-toastification";

const route = useRoute();
const router = useRouter();
const toast = useToast();

onMounted(() => {
  const status = route.query.status;
  const message = decodeURIComponent(route.query.message || "");

  // Hiển thị toast tương ứng
  if (status === "success") {
    toast.success(message || "Thanh toán thành công!");
  } else {
    toast.error(message || "Thanh toán thất bại!");
  }

  // Redirect về trang chủ sau 2 giây
  setTimeout(() => {
    router.push("/");
  }, 2000);
});
</script>
