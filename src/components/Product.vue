<template>
    <div class='product'>
        <img height="150" :name="name" :src="src" :price="price" />
        <h2>{{ name }}</h2>
        <p :style="{ color: 'green' }">{{ price }}€</p>
        <button @click="addToCart(id)">Add</button>
        <button @click="removeFromCart(id)">Remove</button>
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