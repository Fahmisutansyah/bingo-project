<template>
  <div class="home">
    <div class='container'>
      <div class='row d-flex justify-content-center'>
        <h1>
          <span>B</span>
          <span>I</span>
          <span>N</span>
          <span>G</span>
          <span>O</span>
          <span>O</span>
          <span>!</span>
        </h1>
      </div>
      <div class='row d-flex justify-content-center mt-5'>
        <b-form @submit.prevent='register'>
          <b-form-group
            label='Username'
            label-for='username'
            description='This will be shown to your opponents!'
          >
            <b-form-input
              id="username"
              required
              v-model='username'
              placeholder='Enter username..'
            >
            </b-form-input>
          </b-form-group>
        </b-form>
      </div>
    </div>
  </div>
  
</template>
<script>
// @ is an alias to /src

export default {
  name: "home",
  components: {
  },
  data(){
    return {
      username: ''
    }
  },
  methods:{
    register () {
      localStorage.setItem('username', this.username);
      this.$store.commit('register', this.username);
      this.$store.dispatch('getAllRoom');
      this.username = '';
      this.$router.push('/lobby');
    }
  }
};
</script>
<style scoped>
.home {
  width: 100%;  
  height: 100vh;
  background: #FF7777;
  -webkit-font-smoothing: antialiased;
  display: flex;
  justify-content: center;
  align-items: center;
}
  h1 {
  height: 100px;
}

h1 span {
  position: relative;
  top: 20px;
  display: inline-block;
  animation: bounce .3s ease infinite alternate;
  font-family: 'Titan One', cursive;
  font-size: 80px;
  color: #FFF;
  text-shadow: 0 1px 0 #CCC,
               0 2px 0 #CCC,
               0 3px 0 #CCC,
               0 4px 0 #CCC,
               0 5px 0 #CCC,
               0 6px 0 transparent,
               0 7px 0 transparent,
               0 8px 0 transparent,
               0 9px 0 transparent,
               0 10px 10px rgba(0, 0, 0, .4);
}

h1 span:nth-child(2) { animation-delay: .1s; }
h1 span:nth-child(3) { animation-delay: .2s; }
h1 span:nth-child(4) { animation-delay: .3s; }
h1 span:nth-child(5) { animation-delay: .4s; }
h1 span:nth-child(6) { animation-delay: .5s; }
h1 span:nth-child(7) { animation-delay: .6s; }
h1 span:nth-child(8) { animation-delay: .7s; }

@keyframes bounce {
  100% {
    top: -20px;
    text-shadow: 0 1px 0 #CCC,
                 0 2px 0 #CCC,
                 0 3px 0 #CCC,
                 0 4px 0 #CCC,
                 0 5px 0 #CCC,
                 0 6px 0 #CCC,
                 0 7px 0 #CCC,
                 0 8px 0 #CCC,
                 0 9px 0 #CCC,
                 0 50px 25px rgba(0, 0, 0, .2);
  }
}


</style>


