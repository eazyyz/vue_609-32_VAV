<script setup>
import AppHeader from './components/AppHeader.vue';
import AppFooter from './components/AppFooter.vue';
import ProductList from './components/ProductList.vue';
import { ref, onMounted } from 'vue';
import axios from 'axios';
const items = ref([])
const fetchItems = async () => {
  try {
    const {data} = await axios.get("https://128fdf104ac1fc64.mokky.dev/items")
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      isAdded: false
    }))
  } catch (error) {
    console.error(error)
  }
}
onMounted(async ()=>{
  await fetchItems()
})

</script>

<template>
  <body class="flex flex-col justify-between h-dvh">
    <div class="mx-auto w-[1440px] mb-25">
      <AppHeader />
      <main class="pt-10">
        <h1 class="text-[40px] font-bold mb-5">Каталог</h1>
        <ProductList :items="items"/>
      </main>
    </div>
    <AppFooter />
  </body>
</template>

<style scoped></style>
