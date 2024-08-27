<template>
    <div class="cart flex flex-col justify-center items-center">
        <h2 class="text-slate-700 font-bold text-3xl">Your Cart</h2>
        <template v-if="cart.length === 0">
            <p class="text-slate-600 italic">NO PRODUCT</p>
        </template>
        <template v-else>
            <h3 class="text-slate-600 text-2xl font-bold">{{ getTotal }} Product{{ (getTotal <= 1) ? '' : 's' }}<span class="text-black"> |</span> <span class="text-emerald-500">{{ getPrice.toFixed(2) }}€</span></h3>
                    <ul>
                        <li v-for="product in cart">
                            {{ product.name }} x {{ product.quantity }} - <span class="text-emerald-500">{{ product.price.toFixed(2) }}€</span>
                        </li>
                    </ul>
        </template>
    </div>
</template>

<script setup>
defineProps({
    cart: Array,
    getTotal: Number,
    getPrice: Number
})
</script>