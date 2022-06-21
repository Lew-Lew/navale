<template>
  <div class="home">

    <table>
      <tbody>
        <tr v-for="b in board" v-bind:key="b">
          <td v-for="c in b" v-bind:key="c">
            <!-- au click tentative devient true pour afficher ce qu'il y a dans la case -->
            <div v-on:click="c.tentative = true">
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
      board: this.initBoard(),
    };
  },

  methods: {

    initBoard: function(){

      // création de la grille de jeu
      let res = []
      for (let index = 0; index < 10; index++) {
        let array = []
        for (let i = 0; i < 10; i++) {
          array.push(new Cell());
        }
        res.push(array)
      }

      // création d'un bateau de 2 cases placement random
      let randomRow = Math.floor(Math.random() * 9-1) + 1
      let randomColumn = Math.floor(Math.random() * 9-1) + 1
      // res[randomRow][randomColumn].tentative = true
      res[randomRow][randomColumn].boat = true
      let cel2 = Math.floor(Math.random() * 2)
      if (cel2 === 0) {
        // res[randomRow][randomColumn + 1].tentative = true
        res[randomRow][randomColumn + 1].boat = true
      } else  {
        // res[randomRow + 1][randomColumn].tentative = true
        res[randomRow + 1][randomColumn].boat = true
      }

      // création d'un bateau de 3 cases placement random
      // reste à gérer le cas pour être sûr que les bateaux ne tombent pas sur la même case
      let rowCel1Boat3 = Math.floor(Math.random() * 9-1) + 1
      let columnCel1Boat3 = Math.floor(Math.random() * 9-1) + 1
      let cel1Boat3 = res[rowCel1Boat3][columnCel1Boat3]
      // cel1Boat3.tentative = true
      cel1Boat3.boat = true

      let r = Math.floor(Math.random() * 2)
      if (r === 0) {
        let cel2Boat3 = res[rowCel1Boat3][columnCel1Boat3 + 1]
        let cel3Boat3 = res[rowCel1Boat3][columnCel1Boat3 + 2]
        // cel2Boat3.tentative = true
        cel2Boat3.boat = true
        // cel3Boat3.tentative = true
        cel3Boat3.boat = true

      } else  {
        let cel2Boat3 = res[rowCel1Boat3 + 1][columnCel1Boat3]
        let cel3Boat3 = res[rowCel1Boat3 + 2][columnCel1Boat3]
        // cel2Boat3.tentative = true
        cel2Boat3.boat = true
        // cel3Boat3.tentative = true
        cel3Boat3.boat = true
      }
      return res
    },

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
