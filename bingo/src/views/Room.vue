<template>
  <div class="container room">
      <h2>{{ timer }}</h2>
      <h1>{{ hasil }}</h1>
      {{ random }}
    <div class='row'>
      <div class='container' style="padding-left: 250px;padding-right: 250px">
        <div class="row" v-for="(x, i) in 5" :key="i">
          <div class="col-sm ktk m-1 card text-center" v-for="(y, j) in 5" :key="j" @click="marking(i,j)" style="height:100px">{{ arr[i][j] }}</div>
        </div>
      </div>
    </div>
    <div class='row d-flex justify-content-center'>
      <b-form @submit.prevent='pushNum'>
          <b-form-group>
            <label for="name" style="color: black" class='mb-0'>Number</label>
            <b-form-input
              id="number"
              required
              v-model='num'
              type='number'
              min='1'
              max='99'
            >
            </b-form-input>
            <b-button type="submit" variant='primary' size='sm' class='mt-2'>Submit</b-button>
          </b-form-group>
        </b-form>
      </div>
    </div>
</template>

<style>
.ktk {
    /* background-color: aquamarine; */
    height: 150px;
    width: 90px;
    font-size: 70px;
}
.deck{
  padding-left: 0px;
  padding-right: 0px
}
</style>

<script>

function shuffle(array) {
    for (let i = array.length - 1; i > 0; i--) {
        let j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
    }
}

function cekKURMA(num) {
    switch (num) {
        case 0:
            return ""
            break;
        case 1:
            return "K"
            break;
        case 2:
            return "KU"
            break;
        case 3:
            return "KUR"
            break;
        case 4:
            return "KURM"
            break;
        case 5:
            return "KURMA"
            break;

        default:
            break;
    }
}

function cekMax(array) {
    let max = 0
    let tempMax = 0
    for (let i = 0; i < array.length; i++) {
        tempMax = 0;
        for (let j = 0; j < array.length; j++) {
            if (array[i][j] === 'X') {
                tempMax++
            }
        }
        if (tempMax > max) {
            max = tempMax;
        }
    }

    for (let i = 0; i < array.length; i++) {
        tempMax = 0;
        for (let j = 0; j < array.length; j++) {
            if (array[j][i] === 'X') tempMax++;
        }
        if (tempMax > max) {
            max = tempMax;
        }
    }

    tempMax = 0;
    for (let i = 0; i < array.length; i++) {
        if (array[i][i] === 'X') tempMax++;
    }
    if (tempMax > max) {
        max = tempMax;
    }

    tempMax = 0;
    for (let i = 0; i < array.length; i++) {
        if (array[i][4 - i] === 'X') tempMax++;
    }
    if (tempMax > max) {
        max = tempMax;
    }

    return max;
}

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
            counter: 0,
            random: null,
            angka: [],
            hasil: '',
            timer: 60
        }
    },
    computed: {
        max() {
            return cekMax(this.arr);
        }
    },
    created() {
        for (let i = 1; i < 100; i++) {
            this.angka.push(i);
        }

        shuffle(this.angka);

        let initNum = setInterval(() => {
            if(this.timer == 0) {
                this.timer = ''
                clearInterval(initNum);
            } else {
                this.timer--;
            }
        }, 1000)

        setTimeout(() => {
            let game = setInterval(() => {
                console.log('masuk')
                if(this.max == 5 || this.angka.length == 0) {
                    clearInterval(game);
                    this.random = 'selesai';
                } else {
                    this.random  = this.angka.pop();
                }
            }, 4000)  
        }, 60000)

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
            console
            if (this.random == this.arr[row][col]) {
            //make a copy of the row
            const newRow = this.arr[row].slice(0)
            // update the value
            newRow[col] = 'X'
            // update it in the grid
            this.$set(this.arr, row, newRow)

            }

        }
    },
    watch: {
        max(val) {
            this.hasil = cekKURMA(val);
        }
    }
};
</script>

