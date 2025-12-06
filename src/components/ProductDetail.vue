<template>
  <div class="page-container">
    <!-- Breadcrumb Placeholder -->
    <div class="breadcrumbs" v-if="productExists">
      <span>Best Buy</span> <span class="divider">&rsaquo;</span>
      <span>Computers</span> <span class="divider">&rsaquo;</span>
      <span>{{ product.name }}</span>
    </div>

    <div class="product-wrapper" v-if="productExists">
      <!-- Header Section: Title, SKU, Rating -->
      <div class="product-header">
        <h1 class="product-heading">{{ product.name }}</h1>
        <div class="header-meta">
          <div class="model-info">
            <span class="meta-label">Model:</span>
            <span class="meta-value">{{ product.model || "XY-123" }}</span>
            <span class="meta-separator">|</span>
            <span class="meta-label">SKU:</span>
            <span class="meta-value">{{ product.id }}</span>
          </div>
          <div class="rating-stars">
            ★★★★☆ <span class="count">(4,102)</span>
          </div>
        </div>
      </div>

      <!-- Main Content Grid -->
      <div class="product-grid">
        <!-- Left Column: Image & Description -->
        <div class="primary-column">
          <div class="gallery-container">
            <img :src="product.image" :alt="product.name" class="main-image" />
          </div>

          <div class="overview-section">
            <h3 class="section-title">Overview</h3>
            <div class="description-text">
              <p>{{ product.description }}</p>
              <p>
                Features enhanced durability, premium finish, and
                industry-leading performance metrics tailored for professional
                use.
              </p>
            </div>
          </div>
        </div>

        <!-- Right Column: The "Buy Box" Rail -->
        <div class="buy-box-column">
          <div class="buy-box">
            <div class="price-block">
              <span class="currency-symbol">$</span>
              <span class="price-integer">{{ Math.floor(product.price) }}</span>
              <span class="price-decimal">{{
                (product.price % 1).toFixed(2).substring(1)
              }}</span>
            </div>

            <div class="fulfillment-promise">
              <div class="fulfillment-row">
                <span class="icon-check">✓</span>
                <div>
                  <strong>Pickup:</strong>
                  <span class="green-text">Ready in 1 hour</span><br />
                  <span class="sub-text">at Aventura, FL</span>
                </div>
              </div>
              <div class="fulfillment-row">
                <span class="icon-check">✓</span>
                <div>
                  <strong>Shipping:</strong>
                  <span class="green-text">Get it by Tomorrow</span><br />
                  <span class="sub-text">Free for members</span>
                </div>
              </div>
            </div>

            <div class="cart-actions">
              <div class="qty-wrapper">
                <label>Qty</label>
                <input
                  type="number"
                  v-model="quantity"
                  min="1"
                  class="qty-input"
                />
              </div>
              <button @click="addToCart" class="add-to-cart-btn">
                Add to Cart
              </button>
            </div>

            <div class="guarantee-links">
              <p>Best Buy Totaltech™ Support included</p>
              <p>15-Day Return Period</p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- 404 State -->
    <div class="not-found-container" v-else>
      <div class="error-box">
        <span class="error-icon">⚠️</span>
        <h2>We're sorry.</h2>
        <p>
          The product you are looking for was not found (ID:
          {{ $route.params.id }}).
        </p>
        <router-link to="/" class="home-link">Return to Home Page</router-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductDetail",
  props: ["products"],
  data() {
    return {
      quantity: 1,
    };
  },
  computed: {
    product() {
      // Safety check to ensure products is an array before searching
      if (!Array.isArray(this.products)) return null;
      return this.products.find(
        (product) => product.id == this.$route.params.id
      );
    },
    productExists() {
      return !!this.product;
    },
  },
  methods: {
    addToCart() {
      this.$emit("addToCart", {
        productId: this.product.id,
        quantity: this.quantity,
      });
    },
  },
};
</script>

<style scoped>
/* Reset */
* {
  box-sizing: border-box;
}

.page-container {
  font-family: "Human BBY", Arial, sans-serif;
  color: #1d252c;
  background-color: #fff;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 15px;
}

/* Breadcrumbs */
.breadcrumbs {
  font-size: 11px;
  color: #0046be;
  padding: 15px 0;
  cursor: pointer;
}
.breadcrumbs .divider {
  color: #555;
  margin: 0 5px;
}

/* Header Section */
.product-header {
  border-bottom: 1px solid #c5cbd5;
  padding-bottom: 15px;
  margin-bottom: 25px;
}

.product-heading {
  font-size: 24px;
  font-weight: 600;
  margin: 0 0 10px 0;
  line-height: 1.3;
}

.header-meta {
  display: flex;
  align-items: center;
  font-size: 11px;
  gap: 20px;
}

.model-info {
  display: flex;
  gap: 5px;
}

.meta-label {
  font-weight: 700;
}

.meta-separator {
  margin: 0 5px;
  color: #c5cbd5;
}

.rating-stars {
  color: #ffe000;
  text-shadow: 0 0 1px #e0a300;
  font-size: 14px;
}

.rating-stars .count {
  color: #0046be;
  margin-left: 5px;
  font-family: Arial, sans-serif;
}

/* Grid Layout */
.product-grid {
  display: flex;
  gap: 40px;
}

.primary-column {
  flex: 2; /* Takes up 66% width roughly */
}

.buy-box-column {
  flex: 1; /* Takes up 33% width roughly */
  min-width: 300px;
}

/* Image */
.gallery-container {
  background: #f4f6f8;
  padding: 40px;
  text-align: center;
  border-radius: 4px;
  margin-bottom: 40px;
}

.main-image {
  max-width: 100%;
  max-height: 400px;
  object-fit: contain;
  mix-blend-mode: multiply; /* Helps image blend if white bg */
}

/* Overview */
.overview-section {
  background: #f4f6f8;
  padding: 24px;
  border-radius: 4px;
}

.section-title {
  font-size: 18px;
  border-bottom: 1px solid #c5cbd5;
  padding-bottom: 10px;
  margin-top: 0;
}

.description-text {
  font-size: 13px;
  line-height: 1.6;
}

/* Buy Box - The Distinct Right Rail */
.buy-box {
  border: 1px solid #c5cbd5;
  border-radius: 4px;
  padding: 24px;
  background-color: #fff;
}

.price-block {
  font-weight: 700;
  margin-bottom: 20px;
}

.currency-symbol {
  font-size: 16px;
  vertical-align: top;
  position: relative;
  top: 4px;
}

.price-integer {
  font-size: 32px;
}

.price-decimal {
  font-size: 16px;
  vertical-align: top;
  position: relative;
  top: 4px;
}

/* Fulfillment */
.fulfillment-promise {
  margin-bottom: 20px;
}

.fulfillment-row {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
  font-size: 13px;
}

.icon-check {
  color: #1a8603;
  font-weight: bold;
}

.green-text {
  color: #1a8603;
  font-weight: 700;
}

.sub-text {
  color: #555;
  font-size: 11px;
}

/* Cart Actions */
.cart-actions {
  margin-top: 20px;
}

.qty-wrapper {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 10px;
  font-size: 13px;
  font-weight: 700;
}

.qty-input {
  width: 60px;
  padding: 5px;
  border: 1px solid #c5cbd5;
  border-radius: 4px;
}

.add-to-cart-btn {
  background-color: #ffe000;
  color: #040c13;
  width: 100%;
  border: none;
  padding: 12px;
  font-size: 16px;
  font-weight: 700;
  border-radius: 4px;
  cursor: pointer;
  transition: background 0.2s;
}

.add-to-cart-btn:hover {
  background-color: #fff200;
}

.guarantee-links {
  margin-top: 20px;
  text-align: center;
  font-size: 11px;
  color: #0046be;
}

.guarantee-links p {
  margin: 5px 0;
  cursor: pointer;
  text-decoration: underline;
}

/* Not Found / 404 */
.not-found-container {
  padding: 50px;
  text-align: center;
}

.error-box {
  max-width: 600px;
  margin: 0 auto;
}

.error-icon {
  font-size: 48px;
  display: block;
  margin-bottom: 20px;
}

.home-link {
  background: #0046be;
  color: white;
  padding: 10px 20px;
  text-decoration: none;
  border-radius: 4px;
  font-weight: 700;
  display: inline-block;
  margin-top: 20px;
}

/* Mobile Responsiveness */
@media (max-width: 768px) {
  .product-grid {
    flex-direction: column;
  }

  .product-heading {
    font-size: 20px;
  }

  .buy-box {
    border: none;
    padding: 0;
    border-top: 1px solid #c5cbd5;
    padding-top: 20px;
  }
}
</style>
