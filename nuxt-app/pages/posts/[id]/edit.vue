<script setup lang="ts">
import { Post } from 'types'

const router = useRouter()

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

const isPending = ref(false)
const id = ref(post.value?.id)
const title = ref(post.value?.title)
const body = ref(post.value?.body)

async function formSubmit () {
  isPending.value = true
  const { data, error } = await useFetch('https://jsonplaceholder.typicode.com/posts', {
    method: 'POST',
    body: JSON.stringify({
      id: id.value,
      title: title.value,
      body: body.value,
    }),
    headers: {
      'Content-Type': 'application/json',
    },
  })

  isPending.value = false

  if (error.value) {
    console.error(error.value)
    alert(`${error.value?.statusCode}: ${error.value?.statusMessage}`)

    return
  }

  alert('Post updated')
  return router.push(`/posts/${id.value}`)
}

</script>

<template>
  <div>
    <h1>Edit post post<span v-if="title">: {{ title }}</span></h1>
    <form @submit.prevent="formSubmit">
      <label>
        <span>Title</span>
        <input type="text" name="title" v-model.trim="title" :disabled="isPending"/>
      </label>

      <label>
        <span>Body</span>
        <textarea name="body" v-model.trim="body" :disabled="isPending"/>
      </label>

      <button type="submit" :disabled="isPending">Update</button>
    </form>
  </div>
</template>

<style scoped>
label {
  display: block;
}

label span {
  display: block;
}
</style>
