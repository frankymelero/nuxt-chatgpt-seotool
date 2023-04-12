<template>
<div class="app">
  <Navbar/>
<main>
 <slot></slot>
</main>
<Footer/>
</div>
</template>
<script setup>
useHead({
    link: [ 
        {
            rel: "stylesheet",
            href: "./assets/css/main.css"
        }
    ]
})
</script>