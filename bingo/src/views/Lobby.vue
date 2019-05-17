<template>
<div class='lobby'>
    <div class='container pt-5' style='z-index:999;'>
      <div class='row d-flex justify-content-center pt-5'>
        <h2 style="color: white">Create a room!</h2>
      </div>
      <div class='row d-flex justify-content-center'>
        <b-form @submit.prevent='createRoom'>
          <b-form-group>
            <label for="name" style="color: white">Lobby Name:</label>
            <b-form-input
              id="name"
              required
              v-model='lobbyName'
              placeholder='Enter lobby name'
            >
            </b-form-input>
          </b-form-group>
        </b-form>
      </div>
      <div class='row d-flex justify-content-center mt-5'>
        <h5 style="color: white">Or</h5>
      </div>
      <div class='row d-flex justify-content-center mt-5'>
        <h3 style="color: white">Available rooms:</h3>
      </div>
      <div class='row d-flex justify-content-center' style="height: 27vh; overflow:scroll">
        <RoomCard v-for='lobby in rooms' :key='lobby.id' :lobby='lobby' class='col col-sm-3 mx-3 px-0'>
          <b-button variant='info' v-if='lobby.status !== "playing"' @click.prevent='joinRoom(lobby.id)'>Join!</b-button>
        </RoomCard>
      </div>
    </div>  

</div>
</template>

<script>
import RoomCard from '@/components/RoomCard.vue'
import { mapState, mapMutations, mapActions } from 'vuex';
export default {
  computed: {
    ...mapState(['rooms']),
  },
  methods: {
    createRoom () {
      this.$store.dispatch('createRoom', this.roomname);
      localStorage.setItem('room', this.roomname);
      this.roomname = '';
    },
    joinRoom (roomId) {
      this.$store.dispatch('joinRoom', roomId);
    }
  },
  components: {
    RoomCard
  },
  created(){
    this.$store.dispatch('getAllRoom')
  },
  data(){
    return {
      roomname: '',
      // lobbies: [
      //   {
      //     title: 'Kuy sokin maen',
      //     status: 'ready',
      //     users: 3
      //   },
      //   {
      //     title: 'Culun lo semua',
      //     status: 'ready',
      //     users: 3
      //   },
      //   {
      //     title: "HAhahahahaha",
      //     status: 'playing',
      //     users: 3
      //   }
      // ],
      lobbyName: ''
    }
  }
}
</script>



<style scoped>

.context h1{
    text-align: center;
    color: #fff;
    font-size: 50px;
}



.lobby{
  background: #4e54c8;  
    background: -webkit-linear-gradient(to left, #8f94fb, #4e54c8);  
    width: 100%;
    height:100vh; 
}
.circles{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    overflow: hidden;
}

.circles li{
    position: absolute;
    display: block;
    list-style: none;
    width: 20px;
    height: 20px;
    background: rgba(255, 255, 255, 0.2);
    animation: animate 25s linear infinite;
    bottom: -150px;
    
}

.circles li:nth-child(1){
    left: 25%;
    width: 80px;
    height: 80px;
    animation-delay: 0s;
}


.circles li:nth-child(2){
    left: 10%;
    width: 20px;
    height: 20px;
    animation-delay: 2s;
    animation-duration: 12s;
}

.circles li:nth-child(3){
    left: 70%;
    width: 20px;
    height: 20px;
    animation-delay: 4s;
}

.circles li:nth-child(4){
    left: 40%;
    width: 60px;
    height: 60px;
    animation-delay: 0s;
    animation-duration: 18s;
}

.circles li:nth-child(5){
    left: 65%;
    width: 20px;
    height: 20px;
    animation-delay: 0s;
}

.circles li:nth-child(6){
    left: 75%;
    width: 110px;
    height: 110px;
    animation-delay: 3s;
}

.circles li:nth-child(7){
    left: 35%;
    width: 150px;
    height: 150px;
    animation-delay: 7s;
}

.circles li:nth-child(8){
    left: 50%;
    width: 25px;
    height: 25px;
    animation-delay: 15s;
    animation-duration: 45s;
}

.circles li:nth-child(9){
    left: 20%;
    width: 15px;
    height: 15px;
    animation-delay: 2s;
    animation-duration: 35s;
}

.circles li:nth-child(10){
    left: 85%;
    width: 150px;
    height: 150px;
    animation-delay: 0s;
    animation-duration: 11s;
}



@keyframes animate {

    0%{
        transform: translateY(0) rotate(0deg);
        opacity: 1;
        border-radius: 0;
    }

    100%{
        transform: translateY(-1000px) rotate(720deg);
        opacity: 0;
        border-radius: 50%;
    }

}
</style>

