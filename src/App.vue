<template>
  <div id="app">
    <p>Page: {{ this.currentPage }}</p>
    <UserList
      v-bind:users="users"
    />
  </div>
</template>

<script>
import axios from "axios";
import UserList from '@/components/user-list';

export default {
  name: 'App',
  components: {
    UserList,
  },
  mounted() {
    this.getData();

    const userList = document.querySelector('.user-list');
    userList.addEventListener('scroll', () => {

      if(userList.scrollTop + userList.clientHeight >= userList.scrollHeight) {
        if (this.totalPages !== this.currentPage) {
          this.currentPage++;
        } else {
          this.currentPage = 1;
        }

        userList.scrollTop = 0;
        this.getData();
      }
    });
  },
  methods: {
    getData() {
      axios
      .get(`https://reqres.in/api/users?page=${this.currentPage}`)
      .then((response) => {
        this.totalPages = response.data.total_pages;
        this.users = response.data.data;
      });
    }
  },
  data() {
    return {
      users: [],
      currentPage: 1,
      totalPages: 1,
      scrollPosition: 0,
    }
  }
}
</script>

<style scoped>
*,
*::before,
*::after {
  box-sizing: border-box;
}

body {
  position: relative;
}

#app {
  font-family: sans-serif;
}

p {
  text-align: center;
  font-size: 2em;
}

</style>
