<script setup lang="ts">
import { Post } from '~/types'

const route = useRoute()
const page = ref(Number(route.query.page) || 1)

const { data: posts, pending, error } = await useFetch<Post[]>('https://jsonplaceholder.typicode.com/posts', {
  key: page.value.toString(),
  query: computed(() => ({
    _limit: 10,
    _start: 10 * (page.value - 1),
  })),
})

watch(() => route.query, () => page.value = Number(route.query.page) || 1)
watch(() => error.value, () => {
  if (error.value) {
    throw createError({
      statusCode: error.value?.statusCode,
      statusMessage: error.value?.statusMessage,
      fatal: true,
    })
  }
}, { immediate: true })
watch(posts, () => {
  if (!posts.value || posts.value.length < 1) {
    throw createError({
      statusCode: 404,
      statusMessage: 'Posts not found',
      fatal: true,
    })
  }
}, { immediate: true })
</script>

<template>
  <div>
    <NuxtLink to="/posts/create">+ Create new post</NuxtLink>
    <ul v-if="!pending">
      <li v-for="{id, title} in posts" :key="id">
        <NuxtLink :to="`/posts/${id}`">{{ title }}</NuxtLink>
        <NuxtLink :to="`/posts/${id}/edit`">...🖋</NuxtLink>
      </li>
    </ul>
    <div v-else>...</div>
    <NuxtLink :to="`/posts?page=${page - 1}`" v-if="page-1 > 0">&lt;</NuxtLink>
    ...
    <NuxtLink :to="`/posts?page=${page + 1}`">&gt;</NuxtLink>
  </div>
</template>

<style scoped>
li a + a {
  margin-inline-start: 1em;
}
</style>
