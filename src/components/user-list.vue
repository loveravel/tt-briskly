<template>
  <ul class="users__list">
    <UserItem
      v-for="user of users"
      v-bind:user="user"
      v-bind:key="user.name"
      v-on:open-modal="openModal"
    />
  </ul>
</template>

<script>
import UserItem from '@/components/user-item';

export default {
  components: {
    UserItem,
  },
  props: ['users'],
  data() {
    return {
      openedUser: null,
    }
  },
  methods: {
    openModal(user) {
      this.$emit('open-modal', user);
    },
  },
}
</script>

<style scoped>
  .users__list {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 2rem;
    grid-column: 1 / 3;
    width: 100%;
    max-height: 60vh;
    margin: 0;
    padding: 0;
    list-style: none;
  }
</style>
