<template>
  <b-card
    :header="lobby.title"
    header-text-variant="white"
    header-tag="header"
    :header-bg-variant="variant"
    style="max-width: 16rem;"
    class="mt-4"
  >
  <div v-if='lobby' class='container-fluid'>
    <div class='row d-flex justify-content-start'>
      <h6>Players:  {{lobby.users}}/5</h6>
    </div>
    <div class='row d-flex justify-content-start'>
      <h6>Status:  {{lobby.status}}</h6>
    </div> <div class='row d-flex justify-content-start'>
      <b-button variant='info' v-if='lobby.status !== "playing"'>Join!</b-button>
    </div>
  </div>
  </b-card>
</template>

<script>
export default {
  name: 'roomCard',
  props: ['lobby'],
  data(){
    return {
      variant: ''
    }
  },
  created(){
    if(this.lobby.status == 'ready'){
      this.variant = 'primary'
    }else if (this.lobby.status == 'playing'){
      this.variant = 'success'
    }else{
      this.variant = 'info'
    }
  }
}
</script>

