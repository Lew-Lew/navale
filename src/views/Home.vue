<template>
  <div class="home">

    <table>
      <tbody>
        <tr v-for="b in board" v-bind:key="b">
          <td v-for="c in b" v-bind:key="c">
            <div v-on:click="c.tentative = true">
              <div v-if="c.tentative == true">
                {{c.boat}}
              </div>
              <div v-else>~</div>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
    <!-- <table class="">
      <tbody>
        <tr>
          <td class="">1</td>
          <td class="">2</td>
        </tr>
        <tr>
          <td class="">3</td>
          <td class="">4</td>
        </tr>
      </tbody>
    </table> -->
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
      let res = []
      for (let index = 0; index < 10; index++) {
        let array = []
        for (let i = 0; i < 10; i++) {
          array.push(new Cell());
        }
        res.push(array)
      }
      let randomRow = Math.floor(Math.random() * 9-1) + 1
      let randomColumn = Math.floor(Math.random() * 9-1) + 1
      res[randomRow][randomColumn].tentative = true
      let cel2 = Math.floor(Math.random() * 2)
      if (cel2 === 0) {
        res[randomRow][randomColumn + 1].tentative = true
      } else  {
        res[randomRow + 1][randomColumn].tentative = true
      }
      return res
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
  }

  table {
    margin-left: auto;
    margin-right: auto;
  }
</style>
