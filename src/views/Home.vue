<template>
  <div class="home">

    <p v-show="isWin()">You Win!</p>
    <button v-on:click="initBoard()">start</button>
    <p>counter: {{counter}}</p>
    {{win}}
    <table>
      <tbody>
        <tr v-for="b in board" v-bind:key="b">
          <td v-for="c in b" v-bind:key="c">
            <!-- au click tentative devient true pour afficher ce qu'il y a dans la case -->
            <div v-on:click="round(c)">
              <!-- si tentative est true alors on affiche -->
              <div v-if="c.tentative == true">
                <!-- si il y avait un bateau alors on affiche le bateau -->
                <div v-if="c.boat == true">
                  <p>boat</p>
                </div>
                <!-- sinon on signifie l'erreur -->
                <div v-else>X</div>
              </div>
              <!-- si tentative est false alors on affiche rien -->
              <div v-else>~</div>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
class Cell {
  constructor(){
    this.boat = false
    this.tentative = false
  }
}
export default {
  name: "Home",
  data: function () {
    return {
      board: [],
      counter: 0,
      win: 0,
    };
  },

  methods: {

    initBoard() {
      // création de la grille de jeu
      this.counter = 0
      this.win = 0
      this.board = []
      for (let index = 0; index < 10; index++) {
        let array = []
        for (let i = 0; i < 10; i++) {
          array.push(new Cell());
        }
        this.board.push(array)
      }

      this.newBoat(2)
      // création d'un bateau de 2 cases placement random
      let randomRow = Math.floor(Math.random() * 9-1) + 1
      let randomColumn = Math.floor(Math.random() * 9-1) + 1
      this.board[randomRow][randomColumn].tentative = true
      this.board[randomRow][randomColumn].boat = true
      let cel2 = Math.floor(Math.random() * 2)
      if (cel2 === 0) {
        this.board[randomRow][randomColumn + 1].tentative = true
        this.board[randomRow][randomColumn + 1].boat = true
      } else  {
        this.board[randomRow + 1][randomColumn].tentative = true
        this.board[randomRow + 1][randomColumn].boat = true
      }

      // création d'un bateau de 3 cases placement random
      // reste à gérer le cas pour être sûr que les bateaux ne tombent pas sur la même case
      let rowCel1Boat3 = Math.floor(Math.random() * 9-2) + 2
      let columnCel1Boat3 = Math.floor(Math.random() * 9-2) + 2
      let cel1Boat3 = this.board[rowCel1Boat3][columnCel1Boat3]
      // cel1Boat3.tentative = true
      cel1Boat3.boat = true

      let r = Math.floor(Math.random() * 2)
      if (r === 0) {
        let cel2Boat3 = this.board[rowCel1Boat3][columnCel1Boat3 + 1]
        let cel3Boat3 = this.board[rowCel1Boat3][columnCel1Boat3 + 2]
        // cel2Boat3.tentative = true
        cel2Boat3.boat = true
        // cel3Boat3.tentative = true
        cel3Boat3.boat = true

      } else  {
        let cel2Boat3 = this.board[rowCel1Boat3 + 1][columnCel1Boat3]
        let cel3Boat3 = this.board[rowCel1Boat3 + 2][columnCel1Boat3]
        // cel2Boat3.tentative = true
        cel2Boat3.boat = true
        // cel3Boat3.tentative = true
        cel3Boat3.boat = true
      }
    },

    isVertical() {
      return Math.floor(Math.random() * 2) === 0
    },

    // n étant le nombre de case du bateau
    newBoat(n) {
      // TODO: 0 ne devrait pas être possible
      let x1 = Math.floor(Math.random() * 9-(n-1)) + (n-1)
      let y1 = Math.floor(Math.random() * 9-(n-1)) + (n-1)
      console.log(x1, y1)
      this.board[x1][y1].boat = true
      this.board[x1][y1].tentative = true

      if (this.isVertical()) {
        // exemple pour 3 case
        // this.board[x1][y1 + 1].boat = true
        // this.board[x1][y1 + 1].tentative = true
        // this.board[x1][y1 + 2].boat = true
        // this.board[x1][y1 + 2].tentative = true
      }
    },


    isWin() {
      // for (const row of this.board) {
      //   for (const cell of row) {
      //     if (cell.boat && !cell.tentative) {
      //       return false
      //     }
      //   }
      // }
      // return true
      return this.win === 5
    },

    round(cell) {
      if (cell.tentative) {
        return
      }
      this.counter += 1
      if (cell.boat && !cell.tentative) {
        this.win += 1
      }
      cell.tentative = true
    }

  }
};
</script>

<style scoped>
  tr {
    border: 1px solid black;
  }

  td {
    border: 1px solid black ;
    width: 55px;
    height: 55px;
  }

  table {
    margin: auto;
  }
</style>
