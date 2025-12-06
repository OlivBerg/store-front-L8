<template>
  <div class="product-card">
    <div class="product-image-wrapper">
      <img :src="product.image" :alt="product.name" class="product-image" />
    </div>

    <div class="product-content">
      <div class="product-header">
        <router-link :to="`/product/${product.id}`" class="product-title-link">
          <h2 class="product-title">{{ product.name }}</h2>
        </router-link>
        <div class="product-meta">
          <span class="model-sku">Model: {{ product.model || "N/A" }}</span>
          <span class="model-sku">SKU: {{ product.sku || product.id }}</span>
        </div>
        <!-- Mock Rating Section -->
        <div class="product-rating">
          <!-- Use a method to convert number to star string -->
          <span
            class="stars"
            :aria-label="`Rating: ${product.rating} out of 5 stars`"
          >
            {{ getStarString(product.rating) }}
          </span>
          <span class="review-count">({{ product.reviews }})</span>
        </div>
      </div>

      <div class="product-details">
        <div class="product-price-section">
          <p class="price">${{ Number(product.price).toFixed(2) }}</p>
          <span class="saving-badge" v-if="product.discount > 0">
            Save ${{ Number(product.discount).toFixed(2) }}
          </span>
        </div>

        <div class="fulfillment-options">
          <div class="fulfillment-item">
            <span class="icon">store</span>
            <div>
              <strong>Pickup:</strong>
              <span class="fulfillment-text">Ready in 1 hour</span>
            </div>
          </div>
          <div class="fulfillment-item">
            <span class="icon">local_shipping</span>
            <div>
              <strong>Shipping:</strong>
              <span class="fulfillment-text">Free 2-day shipping</span>
            </div>
          </div>
        </div>

        <div class="product-controls">
          <button @click="addToCart" class="add-to-cart-btn">
            <span class="cart-icon">🛒</span> Add to Cart
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductCard",
  props: ["product"],
  data() {
    return {
      quantity: 1,
    };
  },
  methods: {
    addToCart() {
      // Add the product and quantity to the cart
      this.$emit("addToCart", {
        productId: this.product.id,
        quantity: this.quantity,
      });
    },
    getStarString(rating) {
      // Default to 0 if rating is missing
      const score = rating || 0;

      // Create a string of stars based on the score
      // Math.round ensures 4.6 becomes 5 stars, 4.2 becomes 4 stars
      const stars = "★★★★★".slice(0, Math.round(score));
      const empty = "☆☆☆☆☆".slice(0, 5 - Math.round(score));

      return stars + empty;
    },
  },
};
</script>

<style scoped>
/* Reset and Font */
* {
  box-sizing: border-box;
}

.product-card {
  font-family: "Human BBY", Arial, sans-serif;
  border: 1px solid #e0e6ef;
  background: #fff;
  display: flex;
  padding: 24px;
  max-width: 900px;
  margin: 10px auto;
  border-radius: 4px;
}

/* Image Column */
.product-image-wrapper {
  flex: 0 0 200px;
  margin-right: 24px;
  display: flex;
  justify-content: center;
  align-items: flex-start;
}

.product-image {
  width: 100%;
  height: auto;
  object-fit: contain;
  max-height: 200px;
}

/* Content Column */
.product-content {
  flex: 1;
  display: flex;
  flex-direction: column;
}

/* Header Area */
.product-title-link {
  text-decoration: none;
  color: #0046be; /* Best Buy Blue */
}

.product-title-link:hover {
  text-decoration: underline;
}

.product-title {
  font-size: 17px;
  line-height: 1.4;
  font-weight: 400; /* Best Buy uses lighter weights for titles often */
  margin: 0 0 8px 0;
  color: #040c13;
}

.product-meta {
  font-size: 11px;
  color: #555;
  margin-bottom: 8px;
  display: flex;
  gap: 15px;
}

.model-sku {
  color: #1d252c;
}

/* Ratings */
.product-rating {
  font-size: 13px;
  margin-bottom: 16px;
  display: flex;
  align-items: center;
}

.stars {
  color: #ffe000; /* Best Buy Yellow for stars (sometimes replaced by blue in newer designs) */
  /* Forcing a text-shadow to make yellow pop on white */
  text-shadow: 0px 0px 1px #e0a300;
  margin-right: 5px;
  font-size: 16px;
}

.review-count {
  color: #0046be;
}

/* Price Section */
.product-details {
  margin-top: auto;
}

.product-price-section {
  margin-bottom: 16px;
  display: flex;
  align-items: center;
  gap: 10px;
}

.price {
  font-size: 24px;
  font-weight: 700;
  margin: 0;
  color: #1d252c;
}

.saving-badge {
  background-color: #bb0628;
  color: white;
  font-size: 11px;
  padding: 2px 6px;
  border-radius: 2px;
  font-weight: 700;
  text-transform: uppercase;
}

/* Fulfillment */
.fulfillment-options {
  margin-bottom: 16px;
  font-size: 13px;
}

.fulfillment-item {
  display: flex;
  align-items: center;
  margin-bottom: 6px;
  color: #1d252c;
}

.icon {
  font-family: "Material Icons", sans-serif; /* Requires Material Icons or similar */
  font-size: 16px;
  margin-right: 8px;
  color: #1d252c;
  /* Fallback shape if no font loaded */
  display: inline-block;
}

.fulfillment-text {
  color: #555;
}

/* Controls */
.product-controls {
  display: flex;
  align-items: center;
}

.add-to-cart-btn {
  background-color: #ffe000; /* The signature Yellow */
  color: #001e73;
  border: none;
  border-radius: 4px;
  padding: 0 16px;
  height: 40px; /* Best Buy buttons are usually blocky */
  font-size: 15px;
  font-weight: 700;
  cursor: pointer;
  display: flex;
  align-items: center;
  transition: background-color 0.2s;
  width: auto;
  min-width: 150px;
  justify-content: center;
}

.add-to-cart-btn:hover {
  background-color: #fff200;
}

.cart-icon {
  margin-right: 8px;
  font-size: 18px;
}

/* Mobile Responsiveness */
@media (max-width: 600px) {
  .product-card {
    flex-direction: column;
    align-items: center;
    text-align: center;
  }

  .product-image-wrapper {
    margin-right: 0;
    margin-bottom: 16px;
  }

  .product-meta,
  .product-price-section,
  .fulfillment-options {
    justify-content: center;
  }

  .add-to-cart-btn {
    width: 100%;
  }
}
</style>
