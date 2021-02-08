<template>
  <div id="saved-foods-table">
    <table class="table">
      <thead>
      <tr>
        <th>Name</th>
        <th>Calories</th>
      </tr>
      </thead>
      <tbody>
<!--      food row component is binded with data from saved foods array, v-for loops through array-->
<!--      to display each food. when edit table is true, delete button is shown-->
      <food-row
          v-for="savedFood in savedFoods"
          v-bind:savedFood="savedFood"
          v-on:delete-food="deleteFood"
          v-bind:edit="editTable"
          v-bind:key="savedFood.key">
      </food-row>
      </tbody>
    </table>

<!--    when edit table button is clicked, delete buttons show on table rows-->
    <div class="edit-table">
      <img alt="edit table" src="@/assets/edit-table.png" @click="editTable = !editTable" v-model="editTable">
    </div>
  </div>
</template>

<script>
import FoodRow from "./FoodRow";

export default {
  name: "FoodTable",
  components: {
      FoodRow
  },
  props: {
    savedFoods: Array
  },
  data(){
    return {
      editTable: false
    }
  },
  methods: {
    deleteFood(savedFood){
      this.$emit('delete-food', savedFood)
    }}
}
</script>

<style scoped>
#saved-foods-table {
  max-height: 500px;
  overflow: scroll;
  padding: 0 20px 0 20px;
}

table {
  margin-top: 30px;
}

table, th {
  border-collapse: collapse;
  width: 100%;
}

.edit-table{
  display: block;
  text-align: center;
  font-size: small;
  position: absolute;
  right: 0;
  bottom: 0
}

img{
  height: 50px;
}

</style>