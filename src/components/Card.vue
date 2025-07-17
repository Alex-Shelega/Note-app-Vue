<template>
    <section class="profile-card">
      <img src="https://placehold.co/150x150" alt="Profile Picture" class="profile-pic" />
      <h2 class="name">Jane Doe</h2>
      <p class="bio">Web developer passionate about creating interactive and dynamic user experiences.</p>
      <button class="follow-btn">Follow</button>
    </section>

    <h1>From app.vue {{ number }}</h1>
</template>

<script setup>
    import { defineProps } from 'vue';

    let {number} = defineProps(['number'])
</script>

<style scoped>

</style>