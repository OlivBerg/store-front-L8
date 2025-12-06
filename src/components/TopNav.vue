<template>
  <nav class="bby-nav">
    <div class="nav-container">
      <!-- Logo Section -->
      <div class="nav-left">
        <router-link to="/" class="logo-link">
          <span class="logo-text">BEST BUY</span>
          <div class="logo-tag"></div>
        </router-link>
      </div>

      <!-- Links Section -->
      <ul class="nav-links" :class="{ 'nav-links--open': isNavOpen }">
        <li><router-link to="/" @click="closeNav">Products</router-link></li>
        <li>
          <router-link to="/cart" @click="closeNav" class="cart-link">
            Cart
            <span class="cart-badge" v-if="cartItemCount > 0">{{
              cartItemCount
            }}</span>
          </router-link>
        </li>
      </ul>

      <!-- Mobile Hamburger -->
      <button class="hamburger" @click="toggleNav">
        <span class="hamburger-icon"></span>
      </button>
    </div>
  </nav>
</template>

<script>
export default {
  name: "TopNav",
  props: ["cartItemCount"],
  data() {
    return {
      isNavOpen: false,
    };
  },
  methods: {
    toggleNav() {
      this.isNavOpen = !this.isNavOpen;
    },
    closeNav() {
      this.isNavOpen = false;
    },
  },
};
</script>

<style scoped>
/* Core Layout */
.bby-nav {
  background-color: #0046be; /* Best Buy Blue */
  color: #fff;
  font-family: Arial, sans-serif;
  height: 65px;
  position: sticky;
  top: 0;
  z-index: 1000;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.nav-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

/* Logo Styling */
.logo-link {
  color: white;
  text-decoration: none;
  font-weight: 900;
  font-size: 24px;
  display: flex;
  align-items: flex-end;
  letter-spacing: 1px;
}

.logo-tag {
  background-color: #ffe000; /* Best Buy Yellow */
  width: 14px;
  height: 8px;
  margin-left: 3px;
  margin-bottom: 5px;
}

/* Navigation Links */
.nav-links {
  display: flex;
  list-style: none;
  margin: 0;
  padding: 0;
  gap: 30px;
  align-items: center;
}

.nav-links a {
  color: white;
  text-decoration: none;
  font-weight: 700;
  font-size: 15px;
  transition: color 0.2s;
}

.nav-links a:hover {
  color: #ffe000;
  text-decoration: underline;
}

/* Cart Badge */
.cart-link {
  display: flex;
  align-items: center;
  gap: 8px;
}

.cart-badge {
  background-color: #ffe000;
  color: #0046be;
  font-size: 12px;
  font-weight: 900;
  padding: 2px 6px;
  border-radius: 10px;
}

/* Hamburger (Mobile Only) */
.hamburger {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
}

.hamburger-icon {
  display: block;
  width: 25px;
  height: 3px;
  background-color: white;
  position: relative;
}

.hamburger-icon::before,
.hamburger-icon::after {
  content: "";
  position: absolute;
  width: 25px;
  height: 3px;
  background-color: white;
  left: 0;
}

.hamburger-icon::before {
  top: -8px;
}
.hamburger-icon::after {
  top: 8px;
}

/* Responsive Mobile Styles */
@media (max-width: 768px) {
  .hamburger {
    display: block;
  }

  .nav-links {
    display: none; /* Hidden by default */
    position: absolute;
    top: 65px;
    left: 0;
    width: 100%;
    background-color: #003da6; /* Slightly darker blue for dropdown */
    flex-direction: column;
    padding: 20px 0;
    gap: 0;
  }

  .nav-links--open {
    display: flex;
  }

  .nav-links li {
    width: 100%;
    text-align: center;
  }

  .nav-links a {
    display: block;
    padding: 15px;
    border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  }
}
</style>
