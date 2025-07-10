<template>
  <div>
    <HomeHeroSlice />
    <UiStats />
    <HomeArticleSlice />
    <HomePackageSlice />
    <HomeTrainerSlice :trainers="trainers" />

  
  </div>
</template>
<script setup>
import { ref } from "vue";

const { data: plansResponse } = await useApiFetch("/membership-plans");
const { data: testimonialsResponse } = await useApiFetch("/testimonials");
const { data: trainersResponse } = await useApiFetch("/trainers?limit=4");
const { data: articleCategoriesResponse } = await useApiFetch(
  "/article-categories"
);

const plans = ref(plansResponse.value?.data || []);
const testimonials = ref(testimonialsResponse.value?.data || []);
const trainers = ref(trainersResponse.value?.data || []);
const articleCategories = ref(articleCategoriesResponse.value?.data || []);
</script>
