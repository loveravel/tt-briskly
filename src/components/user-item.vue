<template>
  <li>
    <a
      v-on:click="$emit('open-modal', user)"
    >
      <img v-bind:src="user.avatar" alt="Аватар пользователя">
      <p>{{ user.first_name }} {{ user.last_name }}</p>
    </a>
  </li>
</template>

<script>
export default {
  props: ['user'],
}
</script>

<style scoped>
  li {
    border: 2px solid steelblue;
    margin-bottom: 1rem;
  }

  a {
    display: flex;
    align-items: center;
    padding: 2rem;
  }

  img {
    vertical-align: top;
    margin-right: 2rem;
    border-radius: 50%;
  }
</style>
