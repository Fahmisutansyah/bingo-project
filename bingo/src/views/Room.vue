<template>
  <div class="room" style="max-height: 100vh">
    <div class="container-fluid room pt-4">
      <h1 style="color:white">{{ timer }}</h1>
      <h1>{{ hasil }}</h1>
      <h1 style="color:white;font-size:70px">{{ random }}</h1>
      <h3 style="color:white" v-if="myRoom.winner !== ''">The winner is {{ myRoom.winner }}!</h3>
      <div class="row">
        <div class="col col-md-9">
          <div class="container" style="padding-left: 250px;padding-right: 250px">
            <div class="row" v-for="(x, i) in 5" :key="i">
              <div
                class="col-sm ktk m-1 card text-center"
                v-for="(y, j) in 5"
                :key="j"
                @click="marking(i,j)"
                style="height:100px"
              >{{ arr[i][j] }}</div>
            </div>
            <b-form @submit.prevent="pushNum" v-if="counter < 25 && myRoom.status == 'playing'">
              <b-form-group>
                <label for="name" style="color: black" class="mb-0">Number</label>
                <b-form-input id="number" required v-model="num" type="number" min="1" max="99"></b-form-input>
              </b-form-group>
              
            </b-form>
          </div>
          <b-button @click.prevent="back">Back</b-button>
        </div>
        <div class="col col-md">
          <div class="d-flex justify-content-center">
            <h1 style="color:white">Players:</h1>
          </div>
          <div class="d-flex justify-content-center">
            <ul>
              <li
                v-for="(player, index) in myRoom.players"
                :key="index"
                style="color:white"
              >{{ player }}</li>
            </ul>
            <button class="tombol blue" v-if="max === 5" @click.prevent="bingoBtn">BINGO!</button>
          </div>
          <div>
            <b-button
              v-if="myRoom.status === 'ready' && myRoom.players.length > 1"
              variant="success"
              @click.prevent="startGame"
            >Start!</b-button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css?family=Permanent+Marker&display=swap');

html, body {
}
.ktk {
  font-family: 'Permanent Marker', cursive;
  /* background-color: aquamarine; */
  height: 150px;
  width: 90px;
  font-size: 50px;
  margin: auto;
}
.ktk:hover{
  cursor: pointer;
}
.deck {
  padding-left: 0px;
  padding-right: 0px;
}
.room {
  background: #00b894;
  background: -webkit-linear-gradient(to left, #8f94fb, #00b894);
  width: 100%;
  height: 100vh;
}
.tombol {
  -moz-transition: all 0.2s ease-in;
  -o-transition: all 0.2s ease-in;
  -webkit-transition: all 0.2s ease-in;
  transition: all 0.2s ease-in;
}

/* button div */
#buttons {
  padding-top: 50px;
  text-align: center;
}

/* start da css for da buttons */
.tombol {
  border-radius: 5px;
  padding: 15px 25px;
  font-size: 1rem;
  text-decoration: none;
  margin: 20px;
  color: #fff;
  position: relative;
  display: inline-block;
}

.tombol:active {
  transform: translate(0px, 5px);
  -webkit-transform: translate(0px, 5px);
  box-shadow: 0px 1px 0px 0px;
}

.blue {
  background-color: #55acee;
  box-shadow: 0px 5px 0px 0px #3c93d5;
}

.blue:hover {
  background-color: #6fc6ff;
}

.green {
  background-color: #2ecc71;
  box-shadow: 0px 5px 0px 0px #15b358;
}

.green:hover {
  background-color: #48e68b;
}

.red {
  background-color: #e74c3c;
  box-shadow: 0px 5px 0px 0px #ce3323;
}

.red:hover {
  background-color: #ff6656;
}

.purple {
  background-color: #9b59b6;
  box-shadow: 0px 5px 0px 0px #82409d;
}

.purple:hover {
  background-color: #b573d0;
}

.orange {
  background-color: #e67e22;
  box-shadow: 0px 5px 0px 0px #cd6509;
}

.orange:hover {
  background-color: #ff983c;
}

.yellow {
  background-color: #f1c40f;
  box-shadow: 0px 5px 0px 0px #d8ab00;
}

.yellow:hover {
  background-color: #ffde29;
}
</style>

<script>
import db from "@/firebase/init";
function shuffle(array) {
  for (let i = array.length - 1; i > 0; i--) {
    let j = Math.floor(Math.random() * (i + 1));
    [array[i], array[j]] = [array[j], array[i]];
  }
}

function cekBINGO(num) {
  switch (num) {
    case 0:
      return "";
      break;
    case 1:
      return "B";
      break;
    case 2:
      return "BI";
      break;
    case 3:
      return "BIN";
      break;
    case 4:
      return "BING";
      break;
    case 5:
      return "BINGO";
      break;

    default:
      break;
  }
}

function cekMax(array) {
  let max = 0;
  let tempMax = 0;
  for (let i = 0; i < array.length; i++) {
    tempMax = 0;
    for (let j = 0; j < array.length; j++) {
      if (array[i][j] === "X") {
        tempMax++;
      }
    }
    if (tempMax > max) {
      max = tempMax;
    }
  }

  for (let i = 0; i < array.length; i++) {
    tempMax = 0;
    for (let j = 0; j < array.length; j++) {
      if (array[j][i] === "X") tempMax++;
    }
    if (tempMax > max) {
      max = tempMax;
    }
  }

  tempMax = 0;
  for (let i = 0; i < array.length; i++) {
    if (array[i][i] === "X") tempMax++;
  }
  if (tempMax > max) {
    max = tempMax;
  }

  tempMax = 0;
  for (let i = 0; i < array.length; i++) {
    if (array[i][4 - i] === "X") tempMax++;
  }
  if (tempMax > max) {
    max = tempMax;
  }

  return max;
}

export default {
  data() {
    return {
      arr: [
        ["", "", "", "", ""],
        ["", "", "", "", ""],
        ["", "", "", "", ""],
        ["", "", "", "", ""],
        ["", "", "", "", ""]
      ],
      num: "",
      idx: [0, 0],
      counter: 0,
      random: null,
      angka: [],
      hasil: "",
      timer: 60,
      selesai: false
    };
  },
  computed: {
    max() {
      return cekMax(this.arr);
    },
    myRoom() {
      return this.$store.state.myRoom;
    }
  },
  created() {
    if (!localStorage.getItem('username')){
      this.$router.push({path: '/'})
    }else{
      db.collection("rooms")
        .doc(this.$route.params.id)
        .onSnapshot(doc => {
          console.log("onsnapshot");
          if (doc.data().status === "playing" && doc.data().winner === "") {
            this.angka = [...doc.data().angka];
            this.initGame();
          }
          if (doc.data().winner !== "") {
            this.selesai = true;
          }
        });
    }
  },
  methods: {
    bingoBtn() {
      let name = localStorage.getItem("username");
      db.collection("rooms")
        .doc(this.myRoom.id)
        .update({
          winner: name
        });
    },
    initGame() {
      let initNum = setInterval(() => {
        if (this.timer == 0) {
          this.timer = "";
          this.counter = 25;
          clearInterval(initNum);
        } else {
          
          this.timer--;
        }
        if(this.timer === 5){
          this.playSound('http://soundbible.com/mp3/Tick Tock-SoundBible.com-1165545065.mp3')
        }
      }, 1000);

      setTimeout(() => {
        let game = setInterval(() => {
          console.log("masuk");
          if (this.angka.length == 0 || this.selesai) {
            clearInterval(game);
            this.random = "Game Over!";
          } else {
            this.random = this.angka.pop();
          }
        }, 4000);
      }, 63000);
    },
    startGame() {
      db.collection("rooms")
        .doc(this.myRoom.id)
        .update({
          status: "playing"
        });
    },
    back() {
      this.$router.push({ path: "/lobby" });
    },
    pushNum() {
      if (
        this.arr
          .join()
          .split(",")
          .indexOf(this.num) < 0
      ) {
        this.arr[this.idx[0]][this.idx[1]] = this.num;
        this.num = "";
        this.idx[1]++;

        this.counter++;
      } else {
        console.log("udah ada");
      }

      if (this.idx[1] === 5) {
        this.idx[0] += 1;
        this.idx[1] = 0;
      }
    },
    marking(row, col) {
      if (this.random == this.arr[row][col]) {
        this.playSound('http://soundbible.com/mp3/Snap-SoundBible.com-405224438.mp3')
        //make a copy of the row
        const newRow = this.arr[row].slice(0);
        // update the value
        newRow[col] = "X";
        // update it in the grid
        this.$set(this.arr, row, newRow);
      }
    },
    playSound(sound) {
      if (sound) {
        var audio = new Audio(sound);
        audio.play();
      }
    }
  },
  watch: {
    max(val) {
      this.hasil = cekBINGO(val);
    }
  }
};
</script>

