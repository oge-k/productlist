<template>
    <div class="container">
        <h1>Product List</h1>
        <div class="product-grid">
            <ProductCard v-for="product in products" :key="product.name" :product="product"
                @add-to-cart="addToCart" />
        </div>
        <Cart
            :cartItems="cartItems"
             :calculateTotal="calculateTotal"
            @update-quantity="updateQuantity"
            @remove-from-cart="removeFromCart"
            @reset-cart="resetCart"
            @confirm-order="confirmOrder"
            :showCart="showCart"
        />
       <Confirmation :isOpen="isConfirmationModalOpen"  :cartItems="cartItems" :calculateTotal="calculateTotal"  @start-new-order="resetCart" />
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import ProductCard from './productCard.vue';
import Cart from './cart.vue';
import Confirmation from './confirmationModal.vue';

const products = ref([]);
const cartItems = ref([]);
const isConfirmationModalOpen = ref(false);
const showCart = ref(true);

onMounted(async () => {
    try {
      const response = await fetch('/data.json');
      if (!response.ok) {
        throw new Error(`HTTP error! Status: ${response.status}`);
      }
      const jsonData = await response.json();
         products.value = jsonData.map(product => ({
            ...product,
        }));
    } catch (error) {
      console.error('Failed to fetch products', error);
    }
});

function addToCart(product) {
    const existingItem = cartItems.value.find((item) => item.name === product.name)
    if (existingItem) {
        existingItem.quantity++
    } else {
        cartItems.value.push({
            ...product,
           quantity: 1 })
    }
}

function updateQuantity(itemId, change) {
    const item = cartItems.value.find((item) => item.name === itemId)
    if (item) {
        item.quantity += change
        if (item.quantity <= 0) {
            removeFromCart(itemId)
        }
    }
}

function removeFromCart(itemId) {
    cartItems.value = cartItems.value.filter((item) => item.name !== itemId)
}

function confirmOrder() {
    if(cartItems.value.length > 0) {
      isConfirmationModalOpen.value = true;
      showCart.value = false;
    }
}

function closeConfirmationModal() {
    isConfirmationModalOpen.value = false;
}
function resetCart() {
    cartItems.value = [];
    isConfirmationModalOpen.value = false;
    showCart.value = true;
}
function calculateTotal() {
    return cartItems.value.reduce((total, item) => total + item.price * item.quantity, 0).toFixed(2);
}
</script>