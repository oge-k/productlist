<template>
    <div class="cart" v-if="showCart">
        <h2>Your Cart ({{ totalItems }})</h2>
        <ul v-if="cartItems.length > 0">
            <li v-for="item in cartItems" :key="item.name">
                 <picture>
                        <source media="(min-width: 1024px)" :srcset="getAssetUrl(item.name, 'desktop')" />
                        <source media="(min-width: 768px)" :srcset="getAssetUrl(item.name, 'tablet')" />
                        <source media="(min-width: 320px)" :srcset="getAssetUrl(item.name, 'mobile')" />
                        <img :src="getAssetUrl(item.name, 'thumbnail')" :alt="item.name" class="item-image"/>
                 </picture>
                <div class="item-details">
                    <span class="item-name">{{ item.name }}</span><br><br>
                    <span class="item-quantity">({{ item.quantity }}x)</span>
                </div>

                <div class="price-details">
                    <span class="unit-price">@ ${{ item.price }} </span><br>

                    <span class="total-price">${{ item.price * item.quantity }}</span>
                </div>
                <button @click="removeFromCart(item.name)" class="remove-item-button">
              <img src="../assets/images/icon-remove-item.svg" /> </button>
            </li>
        </ul>
        <p v-else>Your cart is empty.</p>
        <div class="total" v-if="cartItems.length > 0">
            Total: ${{ calculateTotal() }}
        </div>
        <div class="cart-actions">
            <button @click="$emit('reset-cart')">Start New Order</button>
            <button @click="confirmOrder">Confirm Order</button>
        </div>
    </div>
</template>

<script setup>
import { defineProps, defineEmits, computed, ref, watch} from 'vue';

const props = defineProps({
    cartItems: {
        type: Array,
        required: true,
    },
    calculateTotal: {
         type: Function,
        required: true
     },
    showCart: {
        type: Boolean,
        required: true,
    }
});
const emit = defineEmits(['remove-from-cart','reset-cart','confirm-order'])

const totalItems = computed(() => props.cartItems.reduce((total, item) => total + item.quantity, 0));

function removeFromCart(itemId) {
  emit('remove-from-cart', itemId)
}

function confirmOrder(){
  emit('confirm-order');
}
const assetUrls = ref({})
watch(
    () => props.cartItems,
    async (newCartItems) => {
        if (newCartItems.length > 0) {
            for (const item of newCartItems) {
                  if (!assetUrls.value[item.name]) {
                    assetUrls.value[item.name] = {
                        thumbnail: await import(`../assets/images/${item.image.thumbnail.split('/').pop()}`),
                        mobile: await import(`../assets/images/${item.image.mobile.split('/').pop()}`),
                        tablet: await import(`../assets/images/${item.image.tablet.split('/').pop()}`),
                         desktop: await import(`../assets/images/${item.image.desktop.split('/').pop()}`),
                    }
                }
            }

        }
    },
    { deep: true }
);

function getAssetUrl(itemName, size) {
    return assetUrls.value[itemName]?.[size]?.default || '';
}
</script>
<style scoped>
.unit-price {
  color:rgb(143, 140, 140);
    font-size: 0.8rem;
}
.total-price {
  color:rgb(11, 12, 15);
    font-weight: bold;
    font-size: 0.9rem;
}
</style>