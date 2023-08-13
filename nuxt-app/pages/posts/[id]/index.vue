<script setup lang="ts">
import { Post } from 'types'

const route = useRoute()
const { data: post, error } = await useFetch<Post>(`https://jsonplaceholder.typicode.com/posts/${route.params.id}`, {
  key: route.params.id.toString(),
})

if (error.value) {
  throw createError({
    statusCode: error.value?.statusCode,
    statusMessage: error.value?.statusMessage,
    fatal: true,
  })
}
</script>

<template>
  <div>
    <NuxtLink :to="`/posts/${post.id}/edit`">Edit post</NuxtLink>
    <h1>{{ post.title }}</h1>
    <p>{{ post.body }}</p>
  </div>
</template>

<style scoped>

</style>
