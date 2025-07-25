<template>
  <NuxtLayout>
    <LoadingSpinner v-if="isPageLoading" />
    <NuxtPage />
  </NuxtLayout>
</template>

<script setup>
import LoadingSpinner from "~/components/global/LoadingSpinner.vue";
import { usePageLoading } from "~/composables/usePageLoading";

const { isPageLoading } = usePageLoading();

const router = useRouter();

router.beforeEach(() => {
  isPageLoading.value = true;
});

router.afterEach(() => {
  setTimeout(() => {
    isPageLoading.value = false;
  }, 300);
});
</script>
