<template>
    <div class="product-card">
        <picture>
            <source media="(min-width: 1024px)" :srcset="desktop" />
            <source media="(min-width: 768px)" :srcset="tablet" />
            <source media="(min-width: 320px)" :srcset="mobile" />
             <img :src="thumbnail" :alt="product.name"/>
        </picture>
        <h3>{{ product.name }}</h3>
        <p>Category: {{ product.category }}</p>
        <div class="price-button-container">
            <span class="price">${{ product.price }}</span>
            <button @click="$emit('add-to-cart', product)">Add to Cart</button>
        </div>
    </div>
</template>

<script setup>
import { defineProps } from 'vue';
import thumbnail from '../assets/images/image-waffle-thumbnail.jpg';
import mobile from '../assets/images/image-waffle-mobile.jpg';
import tablet from '../assets/images/image-waffle-tablet.jpg';
import desktop from '../assets/images/image-waffle-desktop.jpg';
const props = defineProps({
  product: {
    type: Object,
    required: true,
  },
});

defineEmits(['add-to-cart']);

</script>