<template>
<div class="app">
<main>
 <slot></slot>
</main>

<Footer></Footer>
</div>
</template>
<script setup>
useHead({
    title: "NUXT SEO tool",
})
</script>