<template>
  <div class="cart-container">
    <div class="cart-header">
      <h1>Your Cart</h1>
    </div>

    <!-- Main Layout: Items Left, Summary Right -->
    <div class="cart-layout" v-if="hasCartItems">
      <!-- Left Column: Cart Items -->
      <div class="cart-items-column">
        <div
          class="cart-item-card"
          v-for="item in cartItems"
          :key="item.product.id"
        >
          <!-- Image -->
          <div class="item-image">
            <img :src="item.product.image" :alt="item.product.name" />
          </div>

          <!-- Details -->
          <div class="item-details">
            <div class="item-header">
              <h3 class="product-name">{{ item.product.name }}</h3>
              <div class="item-price">
                ${{ Number(item.product.price).toFixed(2) }}
              </div>
            </div>

            <!-- Mock Fulfillment Toggles -->
            <div class="fulfillment-toggles">
              <label class="radio-container">
                <input type="radio" checked disabled />
                <span class="radio-label">Pickup at Ottawa Location</span>
              </label>
              <label class="radio-container">
                <input type="radio" disabled />
                <span class="radio-label">Shipping to k1v0c9</span>
              </label>
            </div>

            <!-- Controls -->
            <div class="item-controls">
              <div class="qty-display">Qty: {{ item.quantity }}</div>
              <div class="action-links">
                <button class="text-link" @click="removeFromCart(item)">
                  Remove
                </button>
                <button class="text-link">Save</button>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Right Column: Order Summary -->
      <div class="order-summary-column">
        <div class="summary-card">
          <h2 class="summary-title">Order Summary</h2>

          <div class="summary-row">
            <span>Original Price</span>
            <span>${{ cartTotal }}</span>
          </div>
          <div class="summary-row">
            <span>Savings - JK dont feel like calculating that</span>
            <span>-$0.00</span>
          </div>
          <div class="summary-row">
            <span>Store Pickup</span>
            <span>Free</span>
          </div>
          <div class="summary-row">
            <span>Estimated Sales Tax</span>
            <span>Calculated in checkout</span>
          </div>

          <hr class="divider" />

          <div class="summary-row total-row">
            <span>Total</span>
            <span>${{ cartTotal }}</span>
          </div>

          <button class="checkout-button" @click="submitOrder">Checkout</button>

          <div class="payment-logos">
            <span class="paypal-text">PayPal</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Empty State -->
    <div class="empty-cart-state" v-else>
      <div class="empty-icon">🛒</div>
      <h2>Your cart is empty</h2>
      <p>Check out our daily deals to find something you'll love.</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "ShoppingCart",
  props: ["cartItems"],
  computed: {
    hasCartItems() {
      return this.cartItems.length > 0;
    },
    cartTotal() {
      if (!this.cartItems.length) return "0.00";
      const total = this.cartItems.reduce((sum, item) => {
        return sum + item.quantity * item.product.price;
      }, 0);
      return total.toFixed(2);
    },
  },
  methods: {
    getItemTotal(item) {
      const quantity = item.quantity;
      const price = item.product.price;
      const total = quantity * price;
      return total.toFixed(2);
    },
    removeFromCart(item) {
      const index = this.cartItems.indexOf(item);
      if (index > -1) {
        this.$emit("removeFromCart", index);
      }
    },
    submitOrder() {
      this.$emit("submitOrder");
    },
  },
};
</script>

<style scoped>
/* Reset & Fonts */
* {
  box-sizing: border-box;
}

.cart-container {
  font-family: "Human BBY", Arial, sans-serif;
  color: #1d252c;
  background-color: #f0f2f4; /* Light grey background specific to cart pages */
  min-height: 100vh;
  padding: 20px;
}

.cart-header h1 {
  font-size: 22px;
  font-weight: 600;
  margin-bottom: 20px;
  max-width: 1100px;
  margin-left: auto;
  margin-right: auto;
}

/* Layout */
.cart-layout {
  display: flex;
  gap: 24px;
  max-width: 1100px;
  margin: 0 auto;
  align-items: flex-start;
}

.cart-items-column {
  flex: 2;
}

.order-summary-column {
  flex: 1;
  min-width: 320px;
}

/* Item Card Styles */
.cart-item-card {
  background: white;
  border: 1px solid #c5cbd5;
  border-radius: 4px;
  padding: 24px;
  margin-bottom: 16px;
  display: flex;
}

.item-image {
  flex: 0 0 100px;
  margin-right: 20px;
}

.item-image img {
  width: 100%;
  height: auto;
  object-fit: contain;
}

.item-details {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.item-header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
}

.product-name {
  font-size: 15px;
  color: #0046be; /* Link Blue */
  margin: 0;
  font-weight: 400;
  line-height: 1.4;
  margin-right: 15px;
}

.item-price {
  font-size: 18px;
  font-weight: 700;
}

/* Mock Radio Inputs */
.fulfillment-toggles {
  margin-bottom: 15px;
}

.radio-container {
  display: flex;
  align-items: center;
  margin-bottom: 5px;
  font-size: 13px;
  color: #1d252c;
}

.radio-container input {
  margin-right: 8px;
}

/* Controls (Remove/Save) */
.item-controls {
  margin-top: auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.qty-display {
  font-size: 13px;
  color: #555;
  background: #f0f2f4;
  padding: 5px 10px;
  border-radius: 4px;
}

.text-link {
  background: none;
  border: none;
  color: #0046be;
  font-size: 13px;
  cursor: pointer;
  margin-left: 15px;
  padding: 0;
}

.text-link:hover {
  text-decoration: underline;
}

/* Order Summary Card */
.summary-card {
  background: white;
  padding: 24px;
  border-radius: 4px;
  /* Usually no border on summary, just shadow or background contrast */
}

.summary-title {
  font-size: 18px;
  font-weight: 600;
  margin-top: 0;
  margin-bottom: 20px;
  border-bottom: 1px solid #c5cbd5;
  padding-bottom: 15px;
}

.summary-row {
  display: flex;
  justify-content: space-between;
  margin-bottom: 12px;
  font-size: 13px;
}

.total-row {
  font-weight: 700;
  font-size: 18px;
  margin-top: 15px;
  margin-bottom: 20px;
}

.divider {
  border: 0;
  border-top: 1px solid #c5cbd5;
  margin: 15px 0;
}

/* Checkout Button */
.checkout-button {
  background-color: #ffe000;
  color: #001e73;
  width: 100%;
  border: none;
  padding: 14px;
  font-size: 16px;
  font-weight: 700;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.checkout-button:hover {
  background-color: #fff200;
}

.payment-logos {
  margin-top: 15px;
  text-align: center;
  font-size: 12px;
  color: #002e6d;
  font-weight: bold;
  font-style: italic;
}

/* Empty State */
.empty-cart-state {
  background: white;
  padding: 60px;
  text-align: center;
  border-radius: 4px;
  max-width: 800px;
  margin: 40px auto;
}

.empty-icon {
  font-size: 48px;
  margin-bottom: 20px;
}

/* Mobile Responsiveness */
@media (max-width: 768px) {
  .cart-layout {
    flex-direction: column;
  }

  .cart-item-card {
    flex-direction: column;
  }

  .item-image {
    margin-bottom: 15px;
    display: flex;
    justify-content: center;
  }

  .order-summary-column {
    width: 100%;
  }
}
</style>
