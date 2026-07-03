<script setup>
import { ref } from 'vue'

const props = defineProps({
  cart: Array,
  subtotal: Number,
  discount: Number,
  tax: Number,
  total: Number
})

const emit = defineEmits(['update-quantity', 'remove-item', 'clear-cart', 'apply-coupon'])

const couponInput = ref('')
const couponError = ref(false)
const couponSuccess = ref(false)

const triggerCouponCheck = () => {
  couponError.value = false
  couponSuccess.value = false
  
  if (!couponInput.value.trim()) return

  const verified = emit('apply-coupon', couponInput.value)
  if (verified) {
    couponSuccess.value = true
  } else {
    couponError.value = true
  }
}
</script>

<template>
  <div class="cart-panel-card">
    <div class="cart-header-row">
      <h2 class="cart-title">Your Order Summary</h2>
      <button v-if="cart.length > 0" class="clear-all-btn" @click="$emit('clear-cart')">Clear Cart</button>
    </div>

    <div v-if="cart.length === 0" class="cart-empty-state">
      <span class="basket-icon">🛒</span>
      <p>Your selection queue is empty.</p>
      <p class="helper-text">Select a professional workshop from the menu to build your training invoice.</p>
    </div>

    <div v-else class="cart-items-feed">
      <div v-for="item in cart" :key="item.id" class="cart-line-item">
        <div class="item-meta-info">
          <span class="item-artwork">{{ item.image }}</span>
          <div>
            <h4>{{ item.title }}</h4>
            <p class="unit-cost-tag">R {{ item.price }} each</p>
          </div>
        </div>

        <div class="item-actions-controls">
          <div class="quantity-stepper-box">
            <button @click="$emit('update-quantity', { id: item.id, quantity: item.quantity - 1 })">-</button>
            <span class="qty-counter">{{ item.quantity }}</span>
            <button @click="$emit('update-quantity', { id: item.id, quantity: item.quantity + 1 })">+</button>
          </div>
          <button class="remove-line-btn" @click="$emit('remove-item', item.id)">Remove</button>
        </div>
      </div>

      <div class="coupon-sub-form">
        <div class="input-inline-group">
          <input 
            v-model="couponInput" 
            type="text" 
            placeholder="Promo Code (e.g. SAVE10)" 
            :disabled="couponSuccess"
          />
          <button :disabled="couponSuccess" @click="triggerCouponCheck">Apply</button>
        </div>
        <p v-if="couponError" class="coupon-msg error-msg">Invalid promotion voucher code.</p>
        <p v-if="couponSuccess" class="coupon-msg success-msg">Voucher code verified! 10% deducted.</p>
      </div>

      <div class="pricing-breakdown-ledger">
        <div class="ledger-row">
          <span>Subtotal</span>
          <span>R {{ subtotal.toLocaleString() }}</span>
        </div>
        <div v-if="discount > 0" class="ledger-row discount-row-highlight">
          <span>Promo Discount</span>
          <span>- R {{ discount.toLocaleString() }}</span>
        </div>
        <div class="ledger-row">
          <span>Estimated Tax (15% VAT)</span>
          <span>R {{ tax.toLocaleString() }}</span>
        </div>
        <div class="ledger-row grand-total-row">
          <span>Grand Total</span>
          <span>R {{ total.toLocaleString() }}</span>
        </div>
      </div>

      <button class="checkout-process-btn" @click="alert('Prototype Submission Event Triggered Successfully.')">
        Proceed to Prototype Registration
      </button>
    </div>
  </div>
</template>

<style scoped>
.cart-panel-card {
  background: var(--panel-bg);
  border: 1px solid var(--border-color);
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
}

.cart-header-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1.5rem;
}

.cart-title {
  font-size: 1.25rem;
  font-weight: 700;
  margin: 0;
}

.clear-all-btn {
  background: none;
  border: none;
  color: var(--accent-red);
  font-size: 0.85rem;
  font-weight: 600;
  cursor: pointer;
  padding: 0;
}

.clear-all-btn:hover {
  text-decoration: underline;
}

.cart-empty-state {
  text-align: center;
  padding: 3rem 1rem;
  color: var(--text-muted);
}

.basket-icon {
  font-size: 2.5rem;
  display: block;
  margin-bottom: 1rem;
}

.cart-empty-state p {
  margin: 0 0 0.5rem 0;
  font-weight: 600;
}

.cart-empty-state .helper-text {
  font-size: 0.85rem;
  font-weight: 400;
}

.cart-items-feed {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.cart-line-item {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  padding-bottom: 1.25rem;
  border-bottom: 1px solid var(--border-color);
}

@media (min-width: 400px) {
  .cart-line-item {
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }
}

.item-meta-info {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.item-artwork {
  font-size: 1.75rem;
}

.item-meta-info h4 {
  margin: 0 0 0.25rem 0;
  font-size: 0.95rem;
  font-weight: 700;
}

.unit-cost-tag {
  margin: 0;
  font-size: 0.85rem;
  color: var(--text-muted);
}

.item-actions-controls {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
}

.quantity-stepper-box {
  display: flex;
  align-items: center;
  border: 1px solid var(--border-color);
  border-radius: 6px;
  background: #f8fafc;
}

.quantity-stepper-box button {
  background: none;
  border: none;
  width: 32px;
  height: 32px;
  font-weight: bold;
  cursor: pointer;
}

.quantity-stepper-box button:hover {
  background-color: #cbd5e1;
}

.qty-counter {
  width: 32px;
  text-align: center;
  font-size: 0.9rem;
  font-weight: 600;
}

.remove-line-btn {
  background: none;
  border: none;
  color: var(--text-muted);
  font-size: 0.8rem;
  cursor: pointer;
}

.remove-line-btn:hover {
  color: var(--accent-red);
}

.coupon-sub-form {
  padding: 0.25rem 0;
}

.input-inline-group {
  display: flex;
  gap: 0.5rem;
}

.input-inline-group input {
  flex: 1;
  padding: 0.5rem 0.75rem;
  border: 1px solid var(--border-color);
  border-radius: 6px;
  font-size: 0.85rem;
}

.input-inline-group button {
  background: var(--primary);
  color: white;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 6px;
  font-weight: 600;
  cursor: pointer;
}

.input-inline-group button:disabled {
  background-color: var(--border-color);
  cursor: not-allowed;
}

.coupon-msg {
  font-size: 0.8rem;
  margin: 0.35rem 0 0 0;
  font-weight: 500;
}

.error-msg { color: var(--accent-red); }
.success-msg { color: var(--accent-green); }

.pricing-breakdown-ledger {
  background: #f8fafc;
  padding: 1rem;
  border-radius: 8px;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.ledger-row {
  display: flex;
  justify-content: space-between;
  font-size: 0.9rem;
  color: var(--text-muted);
}

.discount-row-highlight {
  color: var(--accent-green);
  font-weight: 600;
}

.grand-total-row {
  border-top: 1px solid var(--border-color);
  padding-top: 0.75rem;
  font-size: 1.1rem;
  font-weight: 800;
  color: var(--text-dark);
}

.checkout-process-btn {
  width: 100%;
  background: var(--accent-green);
  color: white;
  border: none;
  padding: 0.9rem;
  border-radius: 6px;
  font-weight: 700;
  font-size: 1rem;
  cursor: pointer;
  box-shadow: 0 2px 4px rgba(16, 185, 129, 0.2);
  transition: opacity 0.2s ease;
}

.checkout-process-btn:hover {
  opacity: 0.9;
}
</style>