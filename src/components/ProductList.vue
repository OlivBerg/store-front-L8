<template>
  <div class="plp-container">
    <!-- Header simulation -->
    <div class="plp-header">
      <h1 class="page-title">
        Computers & Tablets
        <span class="result-count">{{ products.length }} items</span>
      </h1>
      <div class="sort-bar">
        <label>Sort By:</label>
        <select class="sort-select">
          <option>Best Selling</option>
          <option>Price: Low to High</option>
          <option>Price: High to Low</option>
          <option>Top Rated</option>
        </select>
      </div>
    </div>

    <div class="plp-layout">
      <!-- Sidebar Filters Simulation -->
      <aside class="sidebar">
        <div class="filter-group">
          <h3 class="filter-title">Category</h3>
          <ul class="filter-list">
            <li><a href="#">Laptops</a></li>
            <li><a href="#">Desktops</a></li>
            <li><a href="#">Monitors</a></li>
          </ul>
        </div>
        <div class="filter-group">
          <h3 class="filter-title">Price</h3>
          <div class="checkbox-row">
            <input type="checkbox" id="p1" />
            <label for="p1">$500 - $749.99</label>
          </div>
          <div class="checkbox-row">
            <input type="checkbox" id="p2" />
            <label for="p2">$750 - $999.99</label>
          </div>
          <div class="checkbox-row">
            <input type="checkbox" id="p3" />
            <label for="p3">$1000 and up</label>
          </div>
        </div>
        <div class="filter-group">
          <h3 class="filter-title">Current Deals</h3>
          <div class="checkbox-row">
            <input type="checkbox" id="d1" />
            <label for="d1">On Sale</label>
          </div>
          <div class="checkbox-row">
            <input type="checkbox" id="d2" />
            <label for="d2">Free Shipping</label>
          </div>
        </div>
      </aside>

      <!-- Main Product Grid -->
      <main class="product-list-wrapper">
        <!-- Render the ProductCards -->
        <ProductCard
          v-for="product in products"
          :key="product.id"
          :product="product"
          @addToCart="addToCart"
        />

        <!-- Empty State Check -->
        <div v-if="products.length === 0" class="empty-state">
          <p>No products found in this category.</p>
        </div>
      </main>
    </div>
  </div>
</template>

<script>
import ProductCard from "../components/ProductCard";

export default {
  name: "ProductList",
  props: ["products"],
  components: {
    ProductCard,
  },
  methods: {
    addToCart({ productId, quantity }) {
      this.$emit("addToCart", {
        productId: productId,
        quantity: quantity,
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

.plp-container {
  font-family: "Human BBY", Arial, sans-serif;
  max-width: 1300px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f0f2f4; /* Light grey background typical of BBY listing pages */
  min-height: 100vh;
}

/* Header Area */
.plp-header {
  border-bottom: 1px solid #c5cbd5;
  padding-bottom: 15px;
  margin-bottom: 20px;
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
}

.page-title {
  font-size: 24px;
  color: #040c13;
  margin: 0;
  font-weight: 600;
}

.result-count {
  font-size: 13px;
  color: #555;
  font-weight: 400;
  margin-left: 10px;
}

.sort-bar {
  display: flex;
  align-items: center;
  font-size: 13px;
}

.sort-bar label {
  font-weight: 700;
  margin-right: 8px;
}

.sort-select {
  padding: 5px 10px;
  border: 1px solid #c5cbd5;
  border-radius: 4px;
  color: #040c13;
  cursor: pointer;
}

/* Layout Grid */
.plp-layout {
  display: flex;
  gap: 20px;
}

/* Sidebar Styles */
.sidebar {
  flex: 0 0 250px;
  display: none; /* Hidden on mobile by default */
  padding-right: 10px;
}

/* Show sidebar on desktop */
@media (min-width: 900px) {
  .sidebar {
    display: block;
  }
}

.filter-group {
  border-bottom: 1px solid #d3d3d3;
  padding-bottom: 20px;
  margin-bottom: 20px;
}

.filter-title {
  font-size: 15px;
  font-weight: 700;
  margin-bottom: 15px;
  color: #040c13;
}

.filter-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.filter-list li {
  margin-bottom: 10px;
}

.filter-list a {
  text-decoration: none;
  color: #0046be;
  font-size: 13px;
}

.filter-list a:hover {
  text-decoration: underline;
}

.checkbox-row {
  display: flex;
  align-items: center;
  margin-bottom: 8px;
  font-size: 13px;
  color: #1d252c;
}

.checkbox-row input {
  margin-right: 10px;
}

/* Main Content Area */
.product-list-wrapper {
  flex: 1;
}

/* Styling specifically for the injected ProductCards to spacing them out */
.product-list-wrapper > div {
  margin-bottom: 16px; /* Spacing between cards */
}

.empty-state {
  text-align: center;
  padding: 50px;
  background: white;
  border-radius: 4px;
  color: #555;
}

/* Mobile Responsiveness */
@media (max-width: 900px) {
  .plp-header {
    flex-direction: column;
    align-items: flex-start;
  }

  .sort-bar {
    margin-top: 10px;
  }
}
</style>
