<script setup>
import AppHeader from './components/AppHeader.vue';
import AppFooter from './components/AppFooter.vue';
import VCatalog from './components/VCatalog.vue';

</script>

<template>
  <div class="flex flex-col justify-between min-h-dvh">
    <div class="mx-auto w-[1440px] mb-25">
      <AppHeader />
      <main class="pt-10">
        <VCatalog />
      </main>
    </div>
    <AppFooter />
  </div>
</template>

<style scoped></style>
