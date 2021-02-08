<template>

  <transition name="modal">
    <div class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">
          <div class="modal-header">
            <h1>{{ selectedFood.description }}</h1>
          </div>

<!--          details for selected food would be shown here...having issues displaying this data-->
          <div class="modal-body">
            <p>calories{{ }} </p>
            <p>carbs{{ }} </p>
            <p>protein{{  }} </p>
            <p>fat{{  }} </p>
          </div>

<!--          when add button is clicked, add food method is activated and modal is closed-->
          <button class="add-button" v-on:click="addFood()" @click="$emit('close')">ADD FOOD</button>
          <div class="close-button topright">
            <button @click="$emit('close')">X</button>
          </div>

        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: "foodDetails",
  props: {
    selectedFood: Object,
    selectedFoodCalories: Number,
    showFoodSearch: Boolean,
    search: String
  },
  data: function() {
    return {
      foodDetails: {}
    }
  },
  methods: {
    // add food method creates food object from selected foods API data
    addFood(){
      let calories
      let carbs
      let protein
      let fat
      this.selectedFood.foodNutrients.forEach(function (nutrient){
        if(nutrient.nutrientName === 'Energy') {
          calories = nutrient.value
        }
        if(nutrient.nutrientName === 'Protein'){
          protein = nutrient.value
        }
        if(nutrient.nutrientName === 'Carbohydrate, by difference'){
          carbs = nutrient.value
        }
        if(nutrient.nutrientName === 'Total lipid (fat)'){
          fat = nutrient.value
        }
      })
      // food object is created from data
      let food = {
        name: this.selectedFood.description,
        calories: calories,
        protein: protein,
        carbs: carbs,
        fat: fat,
      }
      // food object is emitted to App for use
      this.$emit('add-food', food)
    },
  },
}

</script>
<style scoped>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 300px;
  margin: auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  position: relative;
}


.modal-body {
  margin-top: 30px;
}

.modal-enter {
  opacity: 0;
}

.modal-leave-active {
  opacity: 0;
}

.modal-enter .modal-container,
.modal-leave-active .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}

.modal-header{
  background: #fffafa;
  width: 100%;
  height: 40px;
  display: block;
  text-align: center;
  border-bottom: #b7b5b5 solid 2px;
  position: absolute;
  top: 0;
  overflow: auto;
  left: 0;
  font-size: 10px;
}

.close-button button {
  border: none;
  display: inline-block;
  vertical-align: middle;
  overflow: hidden;
  text-decoration: none;
  text-align: center;
  white-space: nowrap;
  background-color: Transparent;
  background-repeat:no-repeat;
  cursor:pointer;
  outline:none;
}

.topright{
  position: absolute;
  right: 0;
  top: 0;
}

.add-button{
  background: #9ede9b;
  border: none;
  border-radius: 2px;
  position: absolute;
  left: 40%;
}
</style>