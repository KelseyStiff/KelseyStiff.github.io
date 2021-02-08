<template>
  <div>
<!--    modal is vue component that functions as a popup-->
    <transition name="modal">
      <div class="modal-mask">
        <div class="modal-wrapper">
          <div class="modal-container">
            <div class="modal-body">

<!--              user input uses searchAllFoods method to pull up foods matching the query in API-->
              <label for="search-input">
                <input
                    id="search-input"
                    type="text"
                    v-model.trim="search"
                    placeholder="Search foods..."
                    @keyup="searchAllFoods"/>
              </label>

<!--              search results are added to array as user enters queries-->
              <div id="food-search-results-list">
                  <ul class="list">

<!--                    search results are looped through to show each one-->
<!--                    when a search result is clicked, food-details is called to create-->
<!--                    and save selected food into an object-->
                    <li v-for="foodSearchResult in foodSearchResults"
                        v-bind:key="foodSearchResult.key"
                        @click="showModal = true, showFoodDetails(foodSearchResult)">

<!--                      name of each food search result is displayed in a list-->
                      {{ foodSearchResult.description }}
                    </li>
                  </ul>
                </div>

<!--              food details component is an additional modal/popup, when item in search result is clicked,-->
<!--              food details for food is shown-->
              <food-details v-if="showModal"
                            @close="showModal = false"
                            v-bind:selectedFood="selectedFood"
                            v-bind:selected-food-calories="selectedFoodCalories"
                            v-on:add-food="addNewFood">
              </food-details>
            </div>
            <button class="close-button topright" @click="$emit('close')">X</button>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>

import FoodDetails from "./FoodDetails";
export default {
  name: 'food-search',
  components: {
    FoodDetails
  },
  data: function () {
    return {
      showModal: false,
      foodSearchResults: [],
      selectedFood: {},
      selectedFoodCalories: 0,
      search: '',
      URL: 'https://api.nal.usda.gov/fdc/v1/foods/search?',
      APIKEY: 'api_key=4lJM36HZ5LpjaQ7bei6cC4JlX6C5xBqohu96XFXH',
      showFoodSearch: false,
      searchResultUrl: '',
    };
  },
  methods: {
    //searches foods in database from search string input
    searchAllFoods() {

      let query = this.search
      //API url set up
      this.searchResultUrl = this.URL+ this.APIKEY + '&query=' + query
      if (this.search) {
        //fetching data from USDA foods database
        fetch(this.searchResultUrl)
            .then(response => response.json())
            .then(res => {
              //response from database is json with array of foods within
              this.foodSearchResults = res.foods
            });
      } else if(!this.search) {
        this.foodSearchResults = []
      }
      return this.searchResultUrl, this.foodSearchResults
    },
    // assigns selectedFood the search result that is clicked
    showFoodDetails(foodSearchResult){
      this.selectedFood = foodSearchResult
    },
    //add new food takes in food object from food-details component, emits it to APP for use.
    addNewFood(food){
      this.selectedFood = food
      this.$emit('food-added', food)
    },
  },
  created() {
    this.searchAllFoods();
  }
}
</script>
<style scoped>
.modal-mask {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 300px;
  margin: 0 auto;
  padding: 20px 30px;
  background-color: #f57979;
  border-radius: 2px;
  transition: all 0.3s ease;
  overflow-y:auto;
  height: auto;
  max-height: 400px;
  position: relative;
  -webkit-box-shadow: 0 5px 6px -6px #777;
  -moz-box-shadow: 0 5px 6px -6px #777;
  box-shadow: 0 5px 6px -6px #777;
}

ul, li {
  list-style-type: none;
}

#search-input {
  position: sticky;
  margin-left: 25%;
  top: 5px;
}

#food-search-results-list{
  position: relative;
}

.close-button{
  border: none;
  display: inline-block;
  padding: 8px 16px;
  vertical-align: middle;
  overflow: hidden;
  text-decoration: none;
  background-color: inherit;
  text-align: center;
  white-space: nowrap
}

.topright{
  position: absolute;
  right: 0;
  top: 0
}

</style>
