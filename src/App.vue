<template class="">
  <!-- Header -->
  <h1 class="bg-emerald-600 text-white font-extrabold text-5xl text-center mb-3 p-4">MyE-Shop 🛒</h1>
  <main class="flex flex-col">
    <!-- Place for bought products -->
    <Cart :cart="cart" :getTotal="getTotal" :getPrice="getPrice" />
    <!-- Products Store -->
    <div class="products flex flex-wrap justify-center gap-10">

      <Product v-for="product in products" :key="product.id" v-bind="product" :add-to-cart="addToCart"
        :remove-from-cart="removeFromCart" />

    </div>
  </main>
  <!-- Classic footer with &copy; -->
  <footer class="text-slate-400 text-center p-4">
    <p>&copy; 2024 - Loick Cherimont</p>
  </footer>
</template>

<script setup>
import { ref, onMounted, computed, watch } from 'vue';
import Product from './components/Product.vue';
import Cart from './components/Cart.vue';

/** -- Usage -- */
const products = ref(null);
const cart = ref([]);

onMounted(fetchData);

/** Prevent products <ref> to mutate */
watch((products), fetchData)

/**
 * Returns total quanity.
 * 
 * @function getTotal
 * @returns {number} Total number of products that user buys.
 */
const getTotal = computed(() => {
  return cart.value.reduce((acc, product) => acc + product.quantity, 0);
})

/**
 * Returns total price.
 * 
 * @function getPrice
 * @returns {number} Total cost that user has to pay.
 */
const getPrice = computed(() => {
  return cart.value.reduce((acc, product) => acc + product.price, 0);
})


/** -- Functions -- */

/**
* Fetch products from products.json'
* Throw an error if cannot get data
*
* @async fetchData
*/
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

/**
* Add a product into the cart by id.
*
* @function addToCart
* @param {number} id - Id of product to add
*/
function addToCart(id) {
  let product = products.value.find(p => p.id === id);

  /** Product does not exists 
   *  Add it to cart with a new quantity prop. initialized to 1 
   */
  if (!cart.value.find(p => p.id === id)) {
    product.quantity = 1;
    cart.value.push(product);

    /**
     * User selected a product ever in cart
     * Update price and quantity props.
     */
  } else {
    const productCart = cart.value.find(p => p.id === id);
    productCart.quantity++;
    productCart.price += product.price;
  }
}

/**
* Remove a product from cart by id.
*
* @function removeFromCart
* @param {number} id - Id of product to remove.
*/
function removeFromCart(id) {
  let product = products.value.find(p => p.id === id);
  const productToRemove = cart.value.find(p => p.id === id);

  switch (true) {
    /** No product, nothing special. */
    case !productToRemove: // 
      return;
    /** More than one, decrement quantity/price props. */
    case productToRemove.quantity > 1:
      productToRemove.quantity--;
      productToRemove.price -= product.price;
      break;
    /** There is 1 product only, delete it from cart */
    default:
      cart.value = cart.value.filter(p => p.id !== id);
  }
}
</script>