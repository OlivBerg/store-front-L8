<template>
  <div class="app-container">
    <TopNav :cartItemCount="cartItemCount" />

    <div class="main-content">
      <router-view
        :products="products"
        :cartItems="cartItems"
        @addToCart="addToCart"
        @removeFromCart="removeFromCart"
        @submitOrder="submitOrder"
      ></router-view>
    </div>

    <footer class="bby-footer">
      <div class="footer-links">
        <span>Accessibility</span>
        <span>Terms & Conditions</span>
        <span>Privacy</span>
        <span>Interest-Based Ads</span>
      </div>
      <p class="copyright">© 2023 Best Buy. All rights reserved.</p>
    </footer>
  </div>
</template>

<script>
import TopNav from "./components/TopNav.vue";

export default {
  name: "App",
  components: {
    TopNav,
  },
  data() {
    return {
      cartItems: [],
      products: [],
    };
  },
  computed: {
    cartItemCount() {
      return this.cartItems.reduce((total, item) => {
        return total + item.quantity;
      }, 0);
    },
  },
  mounted() {
    this.getProducts();
  },
  methods: {
    getProducts() {
      fetch("/products")
        .then((response) => response.json())
        .then((products) => {
          console.log("success getting proxy products");
          this.products = products;
        })
        .catch((error) => {
          console.log(error);
          // Removed alert for cleaner UX, log instead
          console.error("Error occurred while fetching products");
        });
    },
    addToCart({ productId, quantity }) {
      const existingCartItem = this.cartItems.find(
        (item) => item.product.id == productId
      );
      if (existingCartItem) {
        existingCartItem.quantity += quantity;
      } else {
        const product = this.products.find(
          (product) => product.id == productId
        );
        this.cartItems.push({ product, quantity });
      }
    },
    removeFromCart(index) {
      this.cartItems.splice(index, 1);
    },
    submitOrder() {
      const order = {
        customerId: Math.floor(Math.random() * 10000000000).toString(),
        items: this.cartItems.map((item) => {
          return {
            productId: item.product.id,
            quantity: item.quantity,
            price: item.product.price,
          };
        }),
      };

      console.log(JSON.stringify(order));

      fetch(`/order`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(order),
      })
        .then((response) => {
          console.log(response);
          if (!response.ok) {
            alert("Error occurred while submitting order");
          } else {
            this.cartItems = [];
            alert("Order submitted successfully");
          }
        })
        .catch((error) => {
          console.log(error);
          alert("Error occurred while submitting order");
        });
    },
  },
};
</script>

<style>
/* 
  GLOBAL STYLES 
  These apply to the entire application to override browser defaults
  and set the "Best Buy" theme base.
*/

:root {
  --bby-blue: #0046be;
  --bby-yellow: #ffe000;
  --bby-dark-blue: #001e73;
  --text-primary: #1d252c;
  --bg-color: #f0f2f4;
}

body {
  margin: 0;
  padding: 0;
  background-color: var(--bg-color);
  font-family: "Human BBY", Arial, Helvetica, sans-serif; /* Fallback to standard fonts */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: var(--text-primary);
}

/* Ensure footer stays at bottom */
.app-container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.main-content {
  flex: 1; /* Pushes footer down */
  /* Remove the margin-top 120px as TopNav is likely sticky or sized correctly now */
  width: 100%;
}

/* 
  RESET / BASE ELEMENTS
  Overriding the generic button/input styles to match the brand
*/

button {
  cursor: pointer;
  font-family: inherit;
}

/* Common text link style */
a {
  color: var(--bby-blue);
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

/* 
  FOOTER STYLES 
  Simple, grey background footer typical of corporate sites
*/
.bby-footer {
  background-color: #f4f6f8;
  border-top: 1px solid #c5cbd5;
  color: #555;
  padding: 40px 20px;
  text-align: center;
  margin-top: 40px;
}

.footer-links {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-bottom: 20px;
  font-size: 13px;
  color: var(--bby-blue);
}

.footer-links span {
  cursor: pointer;
}

.footer-links span:hover {
  text-decoration: underline;
}

.copyright {
  font-size: 11px;
  color: #1d252c;
}

/* 
  UTILITY CLASSES
  These might be used by child components if scoped styles aren't enough
*/

/* Quantity Input Standard */
.quantity-input {
  border: 1px solid #c5cbd5;
  border-radius: 4px;
  padding: 8px;
  width: 60px;
  text-align: center;
  font-size: 14px;
}

/* Primary Button Standard (Add to Cart / Checkout) */
button.primary-action {
  background-color: var(--bby-yellow);
  color: var(--bby-dark-blue);
  border: none;
  border-radius: 4px;
  padding: 10px 20px;
  font-weight: 700;
  font-size: 15px;
  transition: background-color 0.2s ease;
}

button.primary-action:hover {
  background-color: #fff200;
}
</style>
