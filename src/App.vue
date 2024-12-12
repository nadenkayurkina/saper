<script setup>
import { ref } from 'vue'
import Bomb from './components/Bomb.vue'
import Flag from './components/Flag.vue'

let countRows = Math.floor(Math.random() * 7) + 3
let countColumns = Math.floor(Math.random() * 7) + 3

const bombs = []
for (let i = 0; i < countRows; i = i + 1) {
  const b = []
  for (let i = 0; i < countColumns; i = i + 1) {
    b.push(0)
  }

  bombs.push(b)
}


let somebombs = Math.floor(Math.random() * 10)

for (let i = 0; i < somebombs; i = i + 1) {
  bombs[Math.floor(Math.random() * bombs.length)][Math.floor(Math.random() * bombs[0].length)] = 9
}



const opened = ref([])

for (let i = 0; i < countRows; i = i + 1) {
  const b = []
  for (let i = 0; i < countColumns; i = i + 1) {
    b.push(0)
  }

  opened.value.push(b)
}

let gameover = false
let win = false

const al = (m) => alert(m)
const cl = (...m) => console.log(...m)

const checkbomb = (i, j, event) => {
  if (gameover == true || win == true) {
    return
  }

  event.preventDefault()

  if (opened.value[i][j] == 9) {
    opened.value[i][j] = 0
  } else {
    opened.value[i][j] = 9
  }

  win = true
  for (let i = 0; i < bombs.length; i = i + 1) {
    for (let j = 0; j < bombs[i].length; j = j + 1) {
      if (bombs[i][j] == 9) {
        if (opened.value[i][j] != 9) {
          win = false
        }
      }

      if (bombs[i][j] != 9) {
        if (opened.value[i][j] == 9) {
          win = false
        }
      }
    }
  }

  if (win == true) {
    for (let i = 0; i < bombs.length; i = i + 1) {
      for (let j = 0; j < bombs[i].length; j = j + 1) {
        if (opened.value[i][j] != 9) {
          opened.value[i][j] = 1
        }
      }
    }
  }
}

const open = (i, j) => {
  if (win == true || opened.value[i][j] == 9) {
    return
  }

  opened.value[i][j] = 1

  if (bombs[i][j] == 9) {
    gameover = true
    for (let i = 0; i < bombs.length; i = i + 1) {
      for (let j = 0; j < bombs[i].length; j = j + 1) {
        opened.value[i][j] = 1
      }
    }
  }

  if (bombs[i][j] == 0) {
    if (i + 1 < bombs.length && j + 1 < bombs[i].length && opened.value[i + 1][j + 1] == 0) {
      opened.value[i + 1][j + 1] = 1
      if (bombs[i + 1][j + 1] == 0) {
        open(i + 1, j + 1)
      }
    }

    if (i + 1 < bombs.lenght && j - 1 >= 0 && opened.value[i + 1][j - 1] == 0) {
      opened.value[i + 1][j - 1] = 1
      if (bombs[i + 1][j - 1] == 0) {
        open(i + 1, j - 1)
      }
    }

    if (i - 1 >= 0 && j + 1 < bombs[i].length && opened.value[i - 1][j + 1] == 0) {
      opened.value[i - 1][j + 1] = 1
      if (bombs[i - 1][j + 1] == 0) {
        open(i - 1, j + 1)
      }
    }

    if (i - 1 >= 0 && j - 1 >= 0 && opened.value[i - 1][j - 1] == 0) {
      opened.value[i - 1][j - 1] = 1
      if (bombs[i - 1][j - 1] == 0) {
        open(i - 1, j - 1)
      }
    }

    if (j + 1 < bombs[i].length && opened.value[i][j + 1] == 0) {
      opened.value[i][j + 1] = 1
      if (bombs[i][j + 1] == 0) {
        open(i, j + 1)
      }
    }

    if (i + 1 < bombs.length && opened.value[i + 1][j] == 0) {
      opened.value[i + 1][j] = 1
      if (bombs[i + 1][j] == 0) {
        open(i + 1, j)
      }
    }

    if (j - 1 >= 0 && opened.value[i][j - 1] == 0) {
      opened.value[i][j - 1] = 1
      if (bombs[i][j - 1] == 0) {
        open(i, j - 1)
      }
    }

    if (i - 1 >= 0 && opened.value[i - 1][j] == 0) {
      opened.value[i - 1][j] = 1
      if (bombs[i - 1][j] == 0) {
        open(i - 1, j)
      }
    }
  }
}

for (let i = 0; i < bombs.length; i = i + 1) {
  for (let j = 0; j < bombs[i].length; j = j + 1) {
    if (bombs[i][j] == 9) {
      if (i - 1 >= 0 && j - 1 >= 0 && bombs[i - 1][j - 1] < 9) {
        bombs[i - 1][j - 1] = bombs[i - 1][j - 1] + 1
      }

      if (i + 1 < bombs.length && j + 1 < bombs[i].length && bombs[i + 1][j + 1] < 9) {
        bombs[i + 1][j + 1] = bombs[i + 1][j + 1] + 1
      }

      if (i - 1 >= 0 && j + 1 < bombs[i].length && bombs[i - 1][j + 1] < 9) {
        bombs[i - 1][j + 1] = bombs[i - 1][j + 1] + 1
      }

      if (j - 1 >= 0 && i + 1 < bombs.length && bombs[i + 1][j - 1] < 9) {
        bombs[i + 1][j - 1] = bombs[i + 1][j - 1] + 1
      }

      if (j + 1 < bombs[i].length && bombs[i][j + 1] < 9) {
        bombs[i][j + 1] = bombs[i][j + 1] + 1
      }

      if (i + 1 < bombs.length && bombs[i + 1][j] < 9) {
        bombs[i + 1][j] = bombs[i + 1][j] + 1
      }

      if (j - 1 >= 0 && bombs[i][j - 1] < 9) {
        bombs[i][j - 1] = bombs[i][j - 1] + 1
      }

      if (i - 1 >= 0 && bombs[i - 1][j] < 9) {
        bombs[i - 1][j] = bombs[i - 1][j] + 1
      }
    }
  }
}
</script>

<template>
  <div class="centering">
    <div class="gameover" v-if="gameover == true">Вы проиграли</div>
    <div class="gameover" v-if="win == true">Вы победили</div>
    <table>
      <tr v-for="(row, i) in bombs" :key="i">
        <td
          v-for="(value, j) in row"
          :class="[
            value == 0 && 'grey',
            value == 1 && 'blue',
            value == 2 && 'green',
            value == 3 && 'brown',
            value == 4 && 'purple',
            value == 9 && 'red',
            'square', 
           opened[i][j]== 1 && 'openedstyle'
          ]"
          :key="j"
          @click="open(i, j)"
          @contextmenu="checkbomb(i, j, $event)"
        >
          <template v-if="opened[i][j] == 1">
            <template v-if="value > 0 && value < 9">
              {{ value }}
            </template>
            <template v-if="value == 9">
              <Bomb></Bomb>
            </template>
          </template>
          <template v-if="opened[i][j] == 9">
            <Flag></Flag>
          </template>
        </td>
      </tr>
    </table>
  </div>
</template>

<style scoped>
.gameover {
  color: darkred;
  font-size: 32px;
}

.centering {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;

  height: 100vh;
}
.grey {
  color: grey;
}

.blue {
  color: blue;
}

.green {
  color: rgb(16, 203, 16);
}

.brown {
  color: rgb(188, 83, 14);
}

.purple {
  color: blueviolet;
}

.red {
  color: red;
}

.td {
  border: 1px solid black;
}

.square {
  width: 50px;
  height: 50px;
  background-color: rgb(242, 242, 242);
  border: 5px solid rgb(179, 179, 179);
  border-radius: 16px;
  margin: 6px;
  cursor: pointer;

  text-align: center;
  font-size: 30px;
}

.square:hover {
  background-color: #d7d7d7;
}

.openedstyle {
  background-color: rgb(255, 255, 255);
}

.square:active {
  background-color: #c2c2c2;
}
</style>
