<template>
    <div class='product w-80 p-4 rounded border flex flex-col gap-y-3'>
        <img :name="name" :src="src" :price="price" :alt="name"/>
        <div class="titles flex justify-between">
            <h2 class="text-slate-700 font-bold text-3xl">{{ name }}</h2>
            <h3 class="text-emerald-500 text-2xl font-bold">{{ price }}€</h3>
        </div>
        <div class="buttons flex justify-around">
            <button @click="addToCart(id)"
                class="bg-emerald-500 px-4 py-2 rounded text-white basis-1/3 border-2 border-emerald-500 transition duration-300 hover:bg-white hover:text-emerald-500" title="Add to cart">Add</button>
            <button @click="removeFromCart(id)"
                class="bg-red-500 px-4 py-2 rounded text-white basis-1/3 border-2 border-red-500 transition duration-300 hover:bg-white hover:text-red-500" title="Remove from cart">Remove</button>
        </div>
    </div>
</template>

<script setup>
defineProps({
    id: Number,
    src: String,
    alt: String,
    name: String,
    price: Number,
    addToCart: Function,
    removeFromCart: Function
})
</script>