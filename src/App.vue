<template>
  <div id="app">
    <header class="page-header">
      <div class="page-header__wrapper wrapper">
        <p class="page-header__name">
          Briskly <sup>test task</sup>
        </p>
        <nav class="page-header__nav">Page {{ this.currentPage }} of {{ this.totalPages }}</nav>
      </div>
    </header>
    <section class="users">
      <div class="users__wrapper wrapper">
        <ModalUser
          v-bind:user="selectedUser"
          v-bind:opened="isModalOpened"
          v-on:close-modal="closeModal"
        />
        <UserList
          v-bind:users="users"
          v-on:open-modal="openModal"
        />
        <div class="users__thumb thumb">
          <div class="thumb__control" v-bind:style="{
            width: 100 / this.totalPages + '%',
            left: 100 / this.totalPages * (this.currentPage - 1) + '%',
          }"></div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";
import UserList from '@/components/user-list';
import ModalUser from '@/components/modal-user';

export default {
  name: 'App',
  components: {
    UserList,
    ModalUser,
  },
  created() {
    this.loadData();
  },
  mounted() {
    const usersBlock = document.querySelector('.users');

    usersBlock.addEventListener('wheel', (evt) => {
      if (evt.deltaY < 0 && this.currentPage !== 1) {
        this.currentPage--;
      } else if (evt.deltaY > 0 && this.currentPage !== this.totalPages) {
        this.currentPage++;
      }

      this.loadData();
    });
  },
  methods: {
    loadData() {
      axios
      .get(`https://reqres.in/api/users?page=${this.currentPage}`)
      .then((response) => {
        this.totalPages = response.data.total_pages;
        this.users = response.data.data;
      });
    },
    openModal(user) {
      if (this.isModalOpened) {
        this.isModalOpened = false;
        setTimeout(() => {
          this.selectedUser = user;
          this.isModalOpened = true;
        }, 350)
      } else {
        this.selectedUser = user;
        this.isModalOpened = true;
      }
    },
    closeModal() {
      this.isModalOpened = false;
      setTimeout(() => {
        this.selectedUser = null;
      }, 350)
    }
  },
  data() {
    return {
      users: [],
      selectedUser: null,
      currentPage: 1,
      totalPages: 1,
      isModalOpened: false,
    }
  }
}
</script>

<style>
*,
*::before,
*::after {
  box-sizing: border-box;
}

html {
  height: 100%;
}

body {
  position: relative;
  margin:0;
  padding:0;
  color: whitesmoke;
  background: linear-gradient(#141e30, #243b55);
	background-size: 100%;
	background-repeat: no-repeat;
}

#app {
  font-family: sans-serif;
}

.wrapper {
  position: relative;
  max-width: 1024px;
  margin: 0 auto;
}

.page-header {
  margin-bottom: 2rem;
}

.page-header__wrapper {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid whitesmoke;
}

.page-header__name {
  font-size: 1.5em;
  position: relative;
  background-image: linear-gradient(90deg, transparent, whitesmoke);
  background-size: 100% 15%;
  background-position: bottom;
  background-repeat: no-repeat;
}

.page-header__name sup {
  font-size: 0.5em;
  color: #03e9f4;
  text-transform: uppercase;
  position: absolute;
  z-index: -1;
  top: -0.8em;
  right: -4em;
  background: #243b55;
  padding: 0.2rem 0.5rem;
}

.users__wrapper {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
}

.users__thumb {
  grid-column: 1 / 3;
  margin-top: 30px;
}

.thumb {
  position: relative;
  height: 10px;
  background-color: whitesmoke;
  border-radius: 5px;
}

.thumb__control {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  background-color: #03e9f4;
  border-radius: inherit;
}

</style>
