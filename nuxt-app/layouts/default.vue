<script setup lang="ts">

</script>

<template>
  <div class="container">
    <header>header</header>
    <aside>
      <ul>
        <li>
          <NuxtLink to="/">index</NuxtLink>
        </li>
        <li>
          <NuxtLink to="/posts">posts</NuxtLink>
        </li>
      </ul>
    </aside>
    <main>
      <slot/>
    </main>
    <footer>footer</footer>
  </div>
</template>

<style scoped>
.container {
  min-height: 100vh;
  display: grid;
  grid-template-columns: 200px 1fr;
  grid-template-rows: 100px 1fr 100px;
}

header,
footer {
  grid-column: span 2;
}

header,
footer,
aside,
main {
  border: 1px solid lightslategray;
}
</style>
