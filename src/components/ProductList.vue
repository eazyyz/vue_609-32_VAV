<script setup>
import VProduct from './VProduct.vue';
defineProps({
    items: Array,
})
</script>
<template>
    <div class="grid grid-cols-5 gap-5">
        <VProduct v-for="item in items" 
        :key="item.id" 
        :imgUrl="item.imgUrl" 
        :title="item.title" 
        :price="item.price.toLocaleString('ru-RU')"/>
    </div>
</template>