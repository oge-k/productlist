<template>
  <div class="modal-overlay" v-if="isOpen">
    <div class="modal-content">
        <img src="../assets/images/icon-order-confirmed.svg" alt="Order Confirmed" class="confirmation-icon"/>
      <h2>Order confirmed</h2>
      <p class="mb-4">We hope you enjoy your meal</p>
      <ul v-if="cartItems.length > 0" class="cart-details">
        <li v-for="item in cartItems" :key="item.name" class="cart-item">
          <div class="item-details">
            <span class="item-name">{{ item.name }}</span>
            <span class="item-quantity">({{ item.quantity }}x)</span>
          </div>
          <div class="price-details">
            <span class="unit-price">${{ item.price }}</span>
            <span class="total-price">${{ item.price * item.quantity }}</span>
          </div>
        </li>
      </ul>
      <p v-else class="empty-cart-message">No items in the cart.</p>
      <div class="total" v-if="cartItems.length > 0">
        Total: ${{ calculateTotal() }}
      </div>
       <button @click="startNewOrder" class="close-button">Start New Order</button>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
  isOpen: {
      type: Boolean,
      required: true,
  },
    cartItems: {
         type: Array,
         required: true,
     },
       calculateTotal: {
         type: Function,
        required: true
     }
});

const emit = defineEmits(['start-new-order']);
 function startNewOrder() {
        emit('start-new-order');
  }
</script>
<style scoped>
.modal-content {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.confirmation-icon {
    display: block;
    width: 80px; /* Adjust size as needed */
    height: 80px;
    margin: 0 auto 20px; /* Center and give some space */
}

h2 {
   margin-bottom: 10px;
}

.mb-4 {
    margin-bottom: 1.5rem !important;
}

.cart-details {
   width: 100%;
    padding: 0;
    margin-bottom: 20px;
    list-style: none;
}

.cart-item {
   display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 0;
     border-bottom: 1px solid #eee;
}

.cart-item:last-child {
  border-bottom: none;
}
.item-details{
    display: flex;
    flex-direction: column;
    align-items: flex-start;
}
.price-details{
    display: flex;
    flex-direction: column;
   align-items: flex-end;
}
.item-quantity{
    font-size: 0.8rem;
     color: #777;
     margin-left: 5px;
}
.unit-price {
  color:rgb(143, 140, 140);
    font-size: 0.8rem;
}
.total-price {
  color:rgb(11, 12, 15);
    font-weight: bold;
    font-size: 0.9rem;
}

.total {
    text-align: right;
    font-weight: bold;
    margin-top: 10px;
    color: #e67e22;
}
.empty-cart-message {
    color: #777;
    font-style: italic;
   margin-bottom: 20px;
}
.close-button{
    background-color: #e67e22; /* orange */
    color: white;
    padding: 10px 15px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
    font-size: 0.9rem;
      margin-top: 15px;
}
.close-button:hover{
  background-color: #d35400;
}
</style>