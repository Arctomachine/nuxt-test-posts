<script setup lang="ts">

const isPending = ref(false)
const title = ref('')
const body = ref('')
const userId = ref(1)

async function formSubmit () {
  isPending.value = true
  const { data, error } = await useFetch('https://jsonplaceholder.typicode.com/posts', {
    method: 'POST',
    body: JSON.stringify({
      title: title.value,
      body: body.value,
      userId: userId.value,
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

  alert('New post created')
  title.value = ''
  body.value = ''
}
</script>

<template>
  <div>
    <h1>Create new post<span v-if="title">: {{ title }}</span></h1>
    <form @submit.prevent="formSubmit">
      <label>
        <span>Title</span>
        <input type="text" name="title" v-model.trim="title" :disabled="isPending"/>
      </label>

      <label>
        <span>Body</span>
        <textarea name="body" v-model.trim="body" :disabled="isPending"/>
      </label>

      <input type="hidden" name="userId" v-model="userId"/>

      <button type="submit" :disabled="isPending">Create</button>
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
