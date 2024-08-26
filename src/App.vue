<template>
  <!-- Header -->
  <h1>E-Shop 🛒</h1>
  <main>

    <!-- Products Store -->
    <div class="products" :style="{ display: 'flex', justifyContent: 'space-around' }">

      <Product 
        v-for="product in products"
        :key="product.id"
        v-bind="product"
        :add-to-cart="addToCart"
        :remove-from-cart="removeFromCart"
      />

    </div>

    <!-- Place for bought products -->
    <Cart 
      :cart="cart"
      :getTotal="getTotal"
      :getPrice="getPrice"
    />
  </main>
</template>

<script setup>
// -- All imports
import { ref, onMounted, computed, watch } from 'vue';

// todo: Re-use after debugging

import Product from './components/Product.vue';
import Cart from './components/Cart.vue';

// -- Reference & reactives
const products = ref(null);
const cart = ref([]);

// -- Lifecycle hooks
// todo: Review + how to write correctly json
// todo: Fetch products from ./products.json
onMounted(fetchData);


// -- Computed
// Computed for total quantity / price
const getTotal = computed(() => {
  return cart.value.reduce((acc, product) => acc + product.quantity , 0);
})

const getPrice = computed(() => {
  return cart.value.reduce((acc, product) => acc + product.price, 0);
})

// -- Watchers
// Prevent products to mutate?
watch((products), fetchData)

// -- Custom functions
async function fetchData() {
  const response = await fetch('./src/assets/products.json', {
    method: 'GET',
    headers: { 'Accept': 'application/json' }
  });

  if (!response.ok) {
    throw Error('Server not reached!');
  }
  const data = await response.json();

  products.value = data;
}

// todo : Re-see how to create a cart in Vue.js
function addToCart(id) {
  let product = products.value.find(p => p.id === id);

  // New product to add
  // Create a quantity prop.
  if (!cart.value.find(p => p.id === id)) {
    product.quantity = 1;
    cart.value.push(product);
  
  // User selected same product
  // Update price and quantity so in cart
  } else {
    const productCart = cart.value.find(p => p.id === id);
    productCart.quantity++;
    productCart.price += product.price;
  }
}

function removeFromCart(id) {
  let product = products.value.find(p => p.id === id);
  const productToRemove = cart.value.find(p => p.id === id);

  switch (true) {
    case !productToRemove: // do not anything if no product
      return;
    case productToRemove.quantity > 1:
      productToRemove.quantity--;
      productToRemove.price -= product.price;
      break;
    default: // there is 1 product only, delete it from cart
      cart.value = cart.value.filter(p => p.id !== id);
  }
}
</script>