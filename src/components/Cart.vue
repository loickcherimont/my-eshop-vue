<template>
    <div class="cart">
        <h2>Your Cart</h2>
        <h3>Products</h3>
        <template v-if="cart.length === 0">
            NO PRODUCT
        </template>
        <template v-else>
            <p>{{ getTotal }} Product{{ (getTotal < 1) ? '' : 's' }} | {{ getPrice.toFixed(2) }}€</p>
                    <ul>
                        <li v-for="product in cart">
                            {{ product.name }} x {{ product.quantity }} - {{ product.price.toFixed(2) }}€
                        </li>
                    </ul>
        </template>
    </div>
</template>

<script setup>
defineProps({
    cart: Array,
    getTotal: Function,
    getPrice: Function
})
</script>