<template>
  <div
    class="modal-user open"
    v-on:mousedown="mousedown"
    v-if="user !== null"
  >
    <div class="modal-user__info">
      <img v-bind:src="user.avatar" alt="Аватар пользователя">
      <ul>
        <li>id: {{ user.id }}</li>
        <li>First Name: {{ user.first_name }}</li>
        <li>Last Name: {{ user.last_name }}</li>
        <li>e-mail: {{ user.email }}</li>
      </ul>
    </div>
    <button type="button" v-on:click="$emit('close-modal')" aria-label="Закрыть окно"></button>
  </div>
  <div v-else class="modal-user"></div>
</template>

<script>
export default {
  props: ['user'],
  methods: {
    mousedown(evt) {
      const modal = document.querySelector('.modal-user');

      var startCoordinateList = {
        x: evt.pageX,
        y: evt.pageY
      };

      function onMouseMove(moveEvt) {
        const shift = {
          x: startCoordinateList.x - moveEvt.pageX,
          y: startCoordinateList.y - moveEvt.pageY
        };

        startCoordinateList = {
          x: moveEvt.pageX,
          y: moveEvt.pageY
        };

        let modalTop = modal.offsetTop - shift.y;
        let modalLeft = modal.offsetLeft - shift.x;

        if (moveEvt.pageY < modal.MIN_Y || modalTop < modal.MIN_Y) {
          modalTop = modal.MIN_Y;
        } else if (moveEvt.pageY > modal.MAX_Y || modalTop > modal.MAX_Y) {
          modalTop = modal.MAX_Y;
        }

        modal.style.top = modalTop + 'px';
        modal.style.left = modalLeft + 'px';

      }

      function onMouseUp() {
        document.removeEventListener('mousemove', onMouseMove);
        document.removeEventListener('mouseup', onMouseUp);
      }

      document.addEventListener('mousemove', onMouseMove);
      document.addEventListener('mouseup', onMouseUp);
    }
  }
}
</script>

<style scoped>
  .modal-user {
    position: fixed;
    bottom: 100%;
    left: 50%;
    transform: translateX(-50%);
    min-width: 600px;
    min-height: 200px;
    padding: 3rem;
    background-color: rgba(0, 0, 0, 0.9);
    color: whitesmoke;
    transition: bottom 0.5s ease-out;
  }

  .modal-user.open {
    bottom: 50%;
    transform: translate(-50%, 50%);
    transition: bottom 0.5s ease-in;
  }


  .modal-user__info {
    display: grid;
    grid-template-columns: auto 1fr;
    gap: 2rem;
  }

  ul {
    list-style: none;
    margin: 0;
    padding: 0;
    align-self: center;
  }

  button {
    position: absolute;
    top: 1rem;
    right: 1rem;
    border: none;
    width: 30px;
    height: 30px;
    transform: rotate(45deg);
    background-color: transparent;
  }

  button::before,
  button::after {
    content: '';
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    width: 100%;
    height: 2px;
    background-color: whitesmoke;
  }

  button::after {
    transform: translate(-50%, -50%) rotate(0.25turn);
  }
</style>
