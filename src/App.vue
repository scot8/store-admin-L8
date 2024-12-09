<template>
  <TopNav />
  <router-view
    :orders="orders"
    :products="products"
    @fetchOrders="fetchOrders"
    @completeOrder="completeOrder"
    @addProductsToList="addProductsToList"
    @updateProductInList="updateProductInList"
    @getProduct="getProduct"
    @getProducts="getProducts"
  ></router-view>
</template>

<script>
import TopNav from './components/TopNav.vue';

const productServiceUrl = "/products/";
const singleProductServiceUrl = "/product/";
const makelineServiceUrl = "/makeline/";

export default {
  name: 'App',
  components: {
    TopNav
  },
  data() {
    return {
      orders: [],
      products: [],
      product: {}
    }
  },
  mounted() {
    this.getProducts();
  },
  methods: {
    async addProductsToList(newProduct) {
      this.products.push(newProduct);
    },
    async updateProductInList(updatedProduct) {
      const index = this.products.findIndex(product => product.id === updatedProduct.id);
      this.products[index] = updatedProduct;
    },
    async getProduct(id) {
      fetch(`${singleProductServiceUrl}${id}`)
        .then(response => response.json())
        .then(product => {
          this.product.id = product.id
          this.product.name = product.name
          this.product.image = product.image
          this.product.description = product.description
          this.product.price = product.price
        })
        .catch(error => {
          console.log(error)
          alert('Error occurred while fetching product')
        })
    },
    async getProducts() {
      fetch(`${productServiceUrl}`)
        .then(response => response.json())
        .then(products => {
          this.products = products
        })
        .catch(error => {
          console.log(error)
          alert('Error occurred while fetching products')
        })
    },
    async fetchOrders() {
      await fetch(`${makelineServiceUrl}order/fetch`)
        .then(response => response.json())
        .then(data => {
          console.log(data)
          if (data) {
            this.orders = data;
          } else {
            console.log('No orders from server');
          }
        })
        .catch(error => console.error(error));
    },
    async completeOrder(orderId) {      
      // get the order and update the status
      let order = this.orders.find(order => order.orderId === orderId);
      order.status = 1;

      let orderObject = JSON.stringify(order)
      console.log(orderObject);

      await fetch(`${makelineServiceUrl}order`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json'
        },
        body: orderObject
      })
        .then(response => {
          if (!response.ok) {
            alert('Error occurred while processing order')
          } else {
            alert('Order successfully processed')
            // remove the order from the list
            this.orders = this.orders.filter(order => order.orderId !== orderId);
            this.$router.go(-1);
          }
        })
        .catch(error => {
          console.log(error)
          alert('Error occurred while processing order')
        })
    }
  },
}
</script>

<style>
body {
  background-color: #1e1e1e; /* Cool dark grey color for dark mode */
  color: #e0e0e0; /* Light text for visibility against the dark background */
  margin: 0;
  padding: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 120px;
}

footer {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: #0a5620;
  color: #fff;
  padding: 1rem;
  margin: 0;
}

nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

ul {
  display: flex;
  list-style: none;
  margin: 0;
  padding: 0;
}

li {
  margin: 0 1rem;
}

a {
  color: #fff;
  text-decoration: none;
}

button {
  padding: 10px;
  background-color: #005f8b;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  height: 42px;
}

.product-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
}

.product-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  margin: 1rem;
  padding: 1rem;
  border: 1px solid #ccc;
  border-radius: 0.5rem;
  background-color: rgba(255, 255, 255, 0.9);
}

.product-card img {
  max-width: 100%;
  margin-bottom: 1rem;
}

.product-card a {
  text-decoration: none;
  color: #333;
}

.product-card h2 {
  font-weight: bold;
  margin-bottom: 0.5rem;
}

.product-card p {
  margin-bottom: 1rem;
}

.product-controls {
  display: flex;
  align-items: center;
  margin-top: 0.5rem;
}

.product-controls p {
  margin-right: 20px;
}

.product-controls button:hover {
  background-color: #005f8b;
}

.product-price {
  font-weight: bold;
  font-size: 1.2rem;
}

.quantity-input {
  width: 50px;
  height: 30px;
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 5px;
  margin-right: 10px;
}

.shopping-cart {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: rgba(255, 255, 255, 0.9);
}

.shopping-cart h2 {
  font-size: 24px;
  margin-bottom: 20px;
}

.shopping-cart-table {
  width: 100%;
  border-collapse: collapse;
}

.shopping-cart-table th,
.shopping-cart-table td {
  padding: 10px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.shopping-cart-table th {
  font-weight: bold;
}

.shopping-cart-table td img {
  display: block;
  margin: 0 auto;
}

.checkout-button {
  margin-top: 20px;
  padding: 10px 20px;
  background-color: #007acc;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.checkout-button:hover {
  background-color: #005f8b;
}
</style>