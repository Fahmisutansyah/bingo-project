<template>
  <div>
    <!-- FORM USERNAME -->
    <form @submit.prevent="register">
      <label for="username">Username</label>
      <input type="text" v-model="username">
      <input type="submit">
    </form>

    <!-- FORM CREATE ROOM -->
    <form @submit.prevent="createRoom">
      <label for="roomname">Roomname</label>
      <input type="text" v-model="roomname">
      <input type="submit">
    </form>

    <!-- LOOPING ROOM -->
    <div class="container">
      <div class="row">
        <div class="col-sm card" v-for="(room, index) in rooms" :key="index" >
          <div class="card body">
            <h5 class="card title">
              {{ room.name }}
            </h5>
            <a @click.prevent="joinRoom(room.id)" class="btn btn-primary">Join</a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState, mapMutations, mapActions } from 'vuex';

export default {
  name: 'home',
  data() {
    return {
      username: '',
      roomname: ''
    }
  },
  components: {
  },
  created() {
    this.$store.dispatch('getAllRoom');
  },
  computed: {
    ...mapState(['rooms']),
  },
  methods: {
    register () {
      localStorage.setItem('username', this.username);
      this.$store.commit('register', this.username);
      this.$store.dispatch('getAllRoom');
      this.username = '';
      // this.$router.push('/lobby');
    },
    
    createRoom () {
      this.$store.dispatch('createRoom', this.roomname);
      localStorage.setItem('room', this.roomname);
      this.roomname = '';
    },
    joinRoom (roomId) {
      this.$store.dispatch('joinRoom', roomId);
    }
  }
}
</script>
