<template>
  <form @submit.prevent="addRecipe">
    <div>
      <label>name</label><br />
      <input v-model="recipe.name" />
    </div>
    <div>
      <label>ingredients</label><br />
      <textarea v-model="recipe.ingredients"></textarea>
    </div>
    <div>
      <label>steps</label><br />
      <textarea v-model="recipe.steps"></textarea>
    </div>
    <button type="submit">add recipe</button>
  </form>
  <div v-for="(r, index) of recipes" :key="r.id">
    <h1>{{ r.name }}</h1>
    <h2>ingredients</h2>
    <div class="content">{{ r.ingredients }}</div>
    <h2>steps</h2>
    <div class="content">{{ r.steps }}</div>
    <button type="button" @click="deleteRecipe(index)">delete</button>
  </div>
</template>

<script>
import { v4 as uuidv4 } from "uuid";
export default {
  name: "App",
  data() {
    return {
      recipe: {
        name: "",
        ingredients: "",
        steps: "",
      },
      recipes: [],
    };
  },
  computed: {
    formValid() {
      const { name, ingredients, steps } = this.recipe;
      return name && ingredients && steps;
    },
  },
  methods: {
    addRecipe() {
      if (!this.formValid) {
        return;
      }
      this.recipes.push({
        id: uuidv4(),
        ...this.recipe,
      });
    },
    deleteRecipe(index) {
      this.recipes.splice(index, 1);
    },
  },
};
</script>

<style scoped>
.content {
  white-space: pre-wrap;
}
</style>