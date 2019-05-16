<template>
  <div class="container">
    <div class="row" v-for="(x, i) in 5" :key="i">
      <div class="col-sm ktk m-1 card text-center" v-for="(y, j) in 5" :key="j" @click="marking(i,j)">{{ arr[i][j] }}</div>
    </div>
    <form @submit.prevent="pushNum" v-if="counter < 25">
        <input type="number" min="0" max="99" required v-model="num">
        <input type="submit">
    </form>
  </div>
</template>

<script>
export default {
    data() {
        return {
            arr: 
                [ 
                    ['', '', '', '', ''],
                    ['', '', '', '', ''],
                    ['', '', '', '', ''],
                    ['', '', '', '', ''],
                    ['', '', '', '', '']
                ],
            num: '',
            idx: [0, 0],
            counter: 0
        }
    },
    computed: {

    },
    created() {
        setTimeout(() => {
            this.arr[0][4] = 'X'
        }, 6000)
    },
    methods: {
        pushNum() {
            
                if (this.arr.join().split(',').indexOf(this.num) < 0) {

                    this.arr[this.idx[0]][this.idx[1]] = this.num;
                    this.num = ''
                    this.idx[1]++

                    this.counter++
                } else {
                    console.log('udah ada')
                }
    
                if (this.idx[1] === 5 ) {
    
                    this.idx[0] += 1;
                    this.idx[1] = 0;

                }

        },
        marking(row, col) {
            //make a copy of the row
            const newRow = this.arr[row].slice(0)
            // update the value
            newRow[col] = 'X'
            // update it in the grid
            this.$set(this.arr, row, newRow)
        }
    }
};
</script>

<style>
.ktk {
    /* background-color: aquamarine; */
    height: 150px;
    width: 90px;
    font-size: 70px;
}
</style>