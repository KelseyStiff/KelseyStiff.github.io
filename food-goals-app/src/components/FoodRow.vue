<template>
  <tr>
<!--    table data from savedFood prop passed from App-->
    <td>{{ savedFood.name }}</td>
    <td>{{ savedFood.calories }}</td>
<!--    delete button activates deleteFood method when clicked-->
    <td v-show="edit"><img v-on:click="deleteFood" src="@/assets/delete.png"></td>
  </tr>
</template>

<script>
export default {
  name: "FoodRow",
  props: {
    savedFood: Object,
    edit: Boolean
  },
  methods: {
    deleteFood(){
      this.$emit('delete-food', this.savedFood)
    }
  }
}
</script>

<style scoped>

img {
  height: 25px;
}

td {
  padding: 5px;
}

tr {
  overflow-y:hidden;
  margin: auto;
}
</style>