<template>
  <div id="app">
    <!--page header-->
    <div class="header">
      <h1 id="logo">Food Goals</h1>
      <div id="add-food-button">
        <!--        add food button, when clicked shows food search box for searching API-->
        <img alt="add food" @click="showFoodSearch = true" src="@/assets/add-food-button.png">
      </div>
    </div> <!-- end of page header-->

<!--    container for macro stats-->
    <div class="macros-container">
      <div class="macros-container-header">
        <h1>Stats</h1>
      </div>

<!--      placeholder for total calories, calculated by computed method-->
      <div id="total-calories">
        <h1>{{ calorieTotal }} <br>
          <span>calories</span></h1>
      </div>
      <!--      placeholder for total carbs, calculated by computed method-->
      <div id="total-carbs">
        <h1>{{ carbTotal | decimalPlaces(0) }} <br>
          <span>carbs</span></h1>
      </div>
      <!--      placeholder for total fat, calculated by computed method-->
      <div id="total-fat">
        <h1>{{ fatTotal | decimalPlaces(0) }} <br>
          <span>fats</span></h1>
      </div>
      <!--      placeholder for total protein, calculated by computed method-->
      <div id="total-protein">
        <h1>{{ proteinTotal | decimalPlaces(0) }} <br>
          <span>protein</span></h1>
      </div>

      <div id="chart-style">
<!--        chart component shows calorie intake, is created when food is added to diary -->
        <macros-charts :chartData="chartData"></macros-charts>
      </div>

    </div>

    <!--table container-->
    <div class="table-container" >
      <div class="table-container-header"><h1>Food Diary</h1></div>
<!--      food diary table is binded with saved foods array, which contains foods saved from api search results-->
<!--      delete-food event emitted from table component triggers foodDelete method to remove food from table-->
      <food-table v-bind:savedFoods="savedFoods" v-on:delete-food="foodDeleted"></food-table>
    </div>


    <div id="notes-container">
      <div class="notes-header">
        <h1>Notes</h1>
      </div>
      <textarea id=notes-input type="text" placeholder="add notes..."></textarea>

    </div> <!--  end of notes container-->

    <!-- food search input container-->
    <div id="food-search">
<!--      food search component shows if add food button is clicked, when food-added even is emitted, -->
<!--      newFoodAdded method add food from search results to table-->
      <food-search v-if="showFoodSearch" @close="showFoodSearch = false" v-on:food-added="newFoodAdded"></food-search>
    </div><!--    end of search input container-->

<!--    credits for images and icons used-->
    <div id="creds">
      Icons made by: <a href="https://www.freepik.com" title="Freepik">Freepik</a> from <a href="https://www.flaticon.com/" title="Flaticon">www.flaticon.com</a>
    </div>

  </div> <!--    end of app container-->
</template>

<script>
// importing vue components
import FoodSearch from './components/FoodSearch.vue'
import FoodTable from "./components/FoodTable";
import MacrosCharts from "./components/MacrosCharts";

export default {
  name: 'App',
  components: {
    FoodSearch,
    FoodTable,
    MacrosCharts,
  },
  data(){
    return {
      savedFoods:  [],
      showFoodSearch: false,
      noData: true,
    }
  },
  methods: {
    newFoodAdded(food){
      // adds food object emitted from child component, adds to savedFoods array
      this.savedFoods.push(food)
    },
    foodDeleted(food){
      //loops through savedFoods array to delete selected food
      this.savedFoods = this.savedFoods.filter( function (f){
        if(f != food){
          return true
        }
      })
    }
  },
  filters:{
    //filters decimal places shown on macro totals
    decimalPlaces(total, numOfDecimal){
        return total.toFixed(numOfDecimal)
    }
  },
  computed: {
    chartData() {
      let labels
      let calories
        labels = this.savedFoods.map(rec => rec.name)
        calories = this.savedFoods.map(rec => rec.calories)
      return {
        labels: labels,
        datasets: [ {
          label: '',
          data: calories,
          backgroundColor: ['#fca8a8','#ffd48a','#7aabc4','#84dd9a', '#8860D0','#8860D0','#C4FFDB','#E483F8','#6AFFC1']
        }],
      }
    },
    calorieTotal: function (){
      //check if saved foods exists, if so calculate total calories
      if(this.savedFoods){
        //learned how to use .reduce method on stack overflow (by: OwChallie)
        return this.savedFoods.reduce(function (prev, cur) {
          return prev + cur.calories;
        }, 0)
      }
    },
    proteinTotal: function (){
      //check if saved foods exists, if so calculate total protein
      if(this.savedFoods){
        return this.savedFoods.reduce(function (prev, cur) {
          return prev + cur.protein;
        }, 0)
      }
    },
    fatTotal: function (){
      //check if saved foods exists, if so calculate total fats
      if(this.savedFoods){
        return this.savedFoods.reduce(function (prev, cur) {
          return prev + cur.fat;
        }, 0)
      }
    },
    carbTotal: function (){
      //check if saved foods exists, if so calculate total carbs
      if(this.savedFoods){
        return this.savedFoods.reduce(function (prev, cur) {
          return prev + cur.carbs;
        }, 0)
      }
    }
  }
}
</script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Lato:wght@300&display=swap');

#app{
  font-family: 'Lato', sans-serif;;
}

body {
  background: #ecc4a5;
  height: auto;
  position: relative;
}

#add-food-button img{
  height: 50px;
  float: right;
  margin-right: 50px;
  padding-bottom: 3px;
}

#logo{
  float: left;
  color: #181818;
  font-size: 40px;
  margin: auto;
  padding-left: 15px;
}

.header{
  background: #fffafa;
  height: 50px;
  margin-bottom: 20px;
  width: 100%;
  -webkit-box-shadow: 0 5px 6px -6px #777;
  -moz-box-shadow: 0 5px 6px -6px #777;
  box-shadow: 0 5px 6px -6px #777;
  border-radius: 3px;
}

.macros-container {
  position: relative;
  background: white;
  max-width: 100%;
  min-width: 550px;
  height: 225px;
  margin: 10px;
  -webkit-box-shadow: 0 8px 6px -6px #777;
  -moz-box-shadow: 0 8px 6px -6px #777;
  box-shadow: 0 8px 6px -6px #777;
}

.macros-container #total-calories h1{
  margin: auto;
  position: absolute;
  top: 35%;
  right: 10%;
  font-size: 50px;
}

.macros-container #total-calories h1 span{
  font-size: 15px;
  position: absolute;
}

.macros-container #total-carbs h1{
  margin: auto;
  position: absolute;
  top: 40%;
  right: 30%;
  font-size: 25px;
}

.macros-container #total-carbs h1 span{
  font-size: 15px;
  position: absolute;
}

.macros-container #total-fat h1 {
  margin: auto;
  position: absolute;
  top: 40%;
  right: 45%;
  font-size: 25px;
}

.macros-container #total-fat h1 span{
  font-size: 15px;
  position: absolute;
}

.macros-container #total-protein h1 {
  margin: auto;
  position: absolute;
  top: 40%;
  right: 60%;
  font-size: 25px;
  display: inline-block;
}

.macros-container #total-protein h1 span{
  font-size: 15px;
  position: absolute;
  left: 20%;
}

.macros-container-header{
  background: #fffafa;
  width: 100%;
  height: 30px;
  display: block;
  text-align: center;
  border-bottom: #b7b5b5 solid 2px;
  position: absolute;
  top: 0;
}

.macros-container-header h1 {
  font-size: 20px;
  margin: auto;
}

.macros-container #chart-style{
  padding: 20px;
  width: 100%;
  max-width: 180px;
  height: 180px;
  position: absolute;
  top: 8%;
}

.table-container {
  float: left;
  background: white;
  min-width: 500px;
  max-width: 50%;
  height: 500px;
  -webkit-box-shadow: 0 8px 6px -6px #777;
  -moz-box-shadow: 0 8px 6px -6px #777;
  box-shadow: 0 8px 6px -6px #777;
  border-radius: 2px;
  position: relative;
  display: block;
  margin: 10px;
  overflow-y:auto;
}

.table-container-header{
  background: #fffafa;
  width: 100%;
  height: 30px;
  display: block;
  text-align: center;
  border-bottom: #b7b5b5 solid 2px;
  position: sticky;
  top: 0;
}

.table-container-header h1{
  font-size: 20px;
  margin: auto;
}

#notes-container{
  float: right;
  background: white;
  width: 40%;
  height: 250px;
  -webkit-box-shadow: 0 8px 6px -6px #777;
  -moz-box-shadow: 0 8px 6px -6px #777;
  box-shadow: 0 8px 6px -6px #777;
  position: relative;
  margin: 10px 10px 0px 0px;
}

.notes-header{
  background: #fffafa;
  width: 100%;
  height: 30px;
  display: block;
  text-align: center;
  border-bottom: #b7b5b5 solid 2px;
  position: absolute;
  top: 0;
}

.notes-header h1 {
  font-size: 20px;
  margin: auto;
}

#notes-input{
  width: 90%;
  height: 70%;
  position: absolute;
  top: 21%;
  border-style: none;
}

::-webkit-input-placeholder {
  position: absolute;
  text-align: left;
}

#creds {
  color: lightgray;
  font-size: x-small;
  bottom: 0;
  left: 75%;
  position: fixed

}

</style>
