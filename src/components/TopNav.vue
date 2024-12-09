<template>
  <nav>
    <div class="logo">
      <a href="/">
        <img src="/BestBuy.png" alt="Algonquin Pet Store Logo">
      </a>
      Admin Portal
    </div>
    <ul class="nav-links" :class="{ 'nav-links--open': isNavOpen }">
      <li><router-link to="/orders" @click="closeNav">Orders</router-link></li>
      <li><router-link to="/products" @click="closeNav">Products</router-link></li>
    </ul>
    <button class="hamburger" @click="toggleNav">
      <span class="hamburger-icon"></span>
    </button>
  </nav>
</template>

<script>
export default {
  name: 'TopNav',
  data() {
    return {
      isNavOpen: false
    }
  },
  methods: {
    toggleNav() {
      this.isNavOpen = !this.isNavOpen
    },
    closeNav() {
      this.isNavOpen = false
    }
  }
}
</script>

<style scoped>
nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #0046be; /* Best Buy blue */
  color: #fff;
  padding: 1rem 2rem; /* Increased padding for a bigger navbar */
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.2);
  font-size: 1.2rem; /* Slightly larger font for better readability */
}

nav img {
  width: 100px; /* Increased logo size */
  height: auto;
}

.nav-links {
  display: flex;
  list-style: none;
  font-size: 1.2rem; /* Larger font size for links */
  font-weight: bold;
  color: #fff;
}

.nav-links a {
  text-decoration: none;
  color: #fff;
  padding: 0 1.5rem; /* Increased padding between links */
  transition: color 0.3s ease;
}

.nav-links a:hover {
  color: #ffd200; /* Best Buy yellow */
}

.hamburger {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0;
  margin: 0;
}

.hamburger-icon {
  display: block;
  width: 30px; /* Increased hamburger icon size */
  height: 4px; /* Increased thickness */
  background-color: #fff;
  position: relative;
  top: 50%;
  transform: translateY(-50%);
}

.hamburger-icon::before,
.hamburger-icon::after {
  content: '';
  display: block;
  width: 30px; /* Matches the width of the main bar */
  height: 4px; /* Matches the thickness of the main bar */
  background-color: #fff;
  position: absolute;
  left: 0;
}

.hamburger-icon::before {
  top: -10px; /* Increased spacing between bars */
}

.hamburger-icon::after {
  bottom: -10px; /* Increased spacing between bars */
}

@media (max-width: 768px) {
  .nav-links {
    display: none;
    position: absolute;
    top: 100%;
    left: 0;
    right: 0;
    background-color: #0046be; /* Same blue as nav */
    padding: 1.5rem; /* More padding for larger touch targets */
    flex-direction: column;
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.3);
  }

  .nav-links--open {
    display: flex;
  }

  .nav-links--open li {
    padding: 1rem 0; /* Increased spacing between dropdown items */
  }

  .hamburger {
    display: block;
  }
}
</style>
