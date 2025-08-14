<template>
  <div>
    <ContactFrom />
    <div class="py-4">
      <div class="text-center bg-gradient-to-br from-gray-950 to-red-950">
        <h2
          class="text-5xl lg:text-6xl font-extrabold mb-4 text-red-400 custom-text-shadow"
        >
          Địa chỉ liên hệ
        </h2>
        <p class="text-xl text-gray-300 max-w-2xl mx-auto mb-8">
          Sẵn sàng bắt đầu hành trình của bạn?
        </p>

        <!-- Bọc iframe trong container responsive -->
        <div id="ban-do" class="w-full max-w-4xl mx-auto aspect-video pb-8">
          <iframe
            class="w-full h-full rounded-lg shadow-lg"
            src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d1442.3313681471902!2d108.16774589360053!3d16.052154954374437!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x31421915a6c92fab%3A0xc894d3afcc1567a8!2zTmd1eeG7hW4gSHV5IFTGsOG7n25nLCBIw7JhIE1pbmgsIExpw6puIENoaeG7g3UsIMSQw6AgTuG6tW5nIDU1MDAwMCwgVmnhu4d0IE5hbQ!5e0!3m2!1svi!2s!4v1755130927694!5m2!1svi!2s"
            style="border: 0"
            allowfullscreen=""
            loading="lazy"
            referrerpolicy="no-referrer-when-downgrade"
          ></iframe>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
useCustomSeoMeta({
  title: "Liên Hệ GymTech",
  description:
    "Liên hệ chúng tôi để được tư vấn các gói tập, huấn luyện viên cá nhân, hoặc bất kỳ hỗ trợ nào.",
  image: "https://gymtech.vn/images/contact.jpg",
  url: "https://gymtech.vn/lien-he",
  keywords:
    "liên hệ phòng gym, tư vấn gói tập, GymTech support, thông tin liên hệ",
});
</script>
