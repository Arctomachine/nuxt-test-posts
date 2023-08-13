<script setup lang="ts">
const props = defineProps({
  error: Object,
})

</script>

<template>
  <NuxtLayout>
    <div>
      <h1>Error {{ error.statusCode }}</h1>
      <p>{{ error.message }}</p>
      <NuxtLink to="/">Back to home page</NuxtLink>
    </div>
  </NuxtLayout>
</template>
