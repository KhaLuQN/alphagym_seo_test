<template>
  <div>
    <HomeHeroSlice />
    <UiStats />
    <HomeArticleSlice />
    <HomePackageSlice :plans="plans" />

    <HomeTrainerSlice :trainers="trainers" />
    <ContactFrom />
  </div>
</template>
<script setup>
import { ref } from "vue";
import { useCustomSeoMeta } from "~/composables/useCustomSeoMeta";

const { data: plansResponse } = await useApiFetch("/membership-plans");
const { data: testimonialsResponse } = await useApiFetch("/testimonials");
const { data: trainersResponse } = await useApiFetch("/trainers?limit=4");

const plans = ref(plansResponse.value?.data || []);
const testimonials = ref(testimonialsResponse.value?.data || []);
const trainers = ref(trainersResponse.value?.data || []);

useCustomSeoMeta({
  title: "AlphaGym - Trung tâm thể hình hiện đại",
  description:
    "Chào mừng bạn đến AlphaGym – phòng gym hàng đầu với huấn luyện viên chuyên nghiệp, máy móc hiện đại, dịch vụ tận tâm.",
  image: "https://alphagym.vn/images/home-banner.jpg",
  url: "https://alphagym.vn",
  keywords: "gym, phòng gym, tập thể hình, huấn luyện viên, AlphaGym",
});
</script>
