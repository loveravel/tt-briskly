<template>
  <li>
    <a
      v-on:click="$emit('open-modal', user)"
    >
      <img v-bind:src="user.avatar" height="50" width="50" alt="Аватар пользователя">
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
    display: flex;
    align-items: center;
    border-left: 2px solid whitesmoke;
  }

  a {
    display: flex;
    align-items: center;
    padding: 1rem 2rem;
    cursor: pointer;
  }

  img {
    vertical-align: top;
    margin-right: 2rem;
    border-radius: 50%;
    border: 2px solid #03e9f4;
  }
</style>
