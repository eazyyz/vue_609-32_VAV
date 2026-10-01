<script setup>
import VProduct from './VProduct.vue';
import {ref, onMounted, computed, watch} from 'vue';
import axios from 'axios';
import SearchInput from './SearchInput.vue';
import SortSelect from './SortSelect.vue';
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
const searchQuery = ref("")
const sortOptions = [
  { value: "default", label: "По умолчанию" },
  { value: "price-asc", label: "Сначала дешевые" },
  { value: "price-desc", label: "Сначала дорогие" },
  { value: "name-asc", label: "Название (А-Я)" },
  { value: "name-desc", label: "Название (Я-А)" },
];
const sortBy = ref(localStorage.getItem('shop_sort_by') || 'default');

watch(sortBy, (newValue) => {
  localStorage.setItem('shop_sort_by', newValue)
})

const filtredProducts = computed(() => {
  let result = [...items.value]

  if (searchQuery.value.trim() !== "") {
    const query = searchQuery.value.toLowerCase();
    result = result.filter((product) =>
      product.title.toLowerCase().includes(query),
    );
  }
  if (sortBy.value === "price-asc") {
    result.sort((a, b) => a.price - b.price);
  } else if (sortBy.value === "price-desc") {
    result.sort((a, b) => b.price - a.price);
  } else if (sortBy.value === "name-asc") {
    result.sort((a, b) =>
    a.title.localeCompare(b.title, ["ru", "en"],{
      sensitivity: "base",
    }));
  }else if (sortBy.value === "name-desc") {
    result.sort((a, b) =>
    b.title.localeCompare(a.title, ["ru", "en"],{
      sensitivity: "base",
    }))
  }
return result;

})

</script>
<template>
    <h1 class="text-[40px] font-bold mb-5">Каталог</h1>
    <div class="flex items-center gap-3 mb-4">
      <SearchInput v-model="searchQuery" placeholder="Поиск по названию..."/>
     <SortSelect v-model="sortBy" :options="sortOptions" />
    </div>

    <div class="grid grid-cols-5 gap-5">
        <VProduct v-for="item in filtredProducts"
        :key="item.id"
        :imgUrl="item.imgUrl"
        :title="item.title"
        :price="item.price.toLocaleString('ru-RU')"/>
      <p v-if="filtredProducts.length === 0">Товары не найдены.</p>
    </div>
</template>
