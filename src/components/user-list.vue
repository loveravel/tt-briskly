<template>
  <div>
    <ul class="user-list">
      <UserItem
        v-for="user of users"
        v-bind:user="user"
        v-bind:key="user.name"
        v-on:open-modal="openModal"
      />
    </ul>

    <ModalUser
      v-bind:user="openedUser"
      v-on:close-modal="closeModal"
    />
  </div>
</template>

<script>
import UserItem from '@/components/user-item';
import ModalUser from '@/components/modal-user';

export default {
  components: {
    UserItem,
    ModalUser,
  },
  props: ['users'],
  data() {
    return {
      openedUser: null,
    }
  },
  methods: {
    openModal(user) {
      this.openedUser = user;
    },
    closeModal() {
      document.querySelector('.modal-user').classList.remove('open');
      setTimeout(() => {
        this.openedUser = null;
      }, 500);
    }
  },
}
</script>

<style scoped>
  ul {
    display: grid;
    gap: 0.2rem;
    list-style: none;
    margin: 0;
    padding: 0;
    max-width: 768px;
    margin: 0 auto;
    max-height: 80vh;
    overflow-y: scroll;
  }
</style>
