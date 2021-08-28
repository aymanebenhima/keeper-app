<template>
  <div class="add-keep container">
    <h2 class="center-align indigo-text">Add new keep</h2>
    <form @submit.prevent="addKeep">
      <div class="field title">
        <label for="title">Keep Title:</label>
        <input type="text" name="title" v-model="title" />
      </div>
      <div class="field" v-for="(ing, index) in ingredients" :key="index">
        <label for="item">Item: {{ index + 1 }}</label>
        <input type="text" name="item" v-model="ingredients[index]" />
        <i class="material-icons delete" @click="deleteItem(ing)">delete</i>
      </div>
      <div class="filed add-keep">
        <label for="add-keep">Add keep and (press SPACE):</label>
        <input
          @keydown.space.prevent="addItem"
          v-model="another"
          type="text"
          name="add-keep"
        />
      </div>
      <div class="field center-align">
        <p v-if="feedback" class="red-text">{{ feedback }}</p>
        <button class="btn pink">Add keep</button>
      </div>
    </form>
  </div>
</template>

<script>
import db from "@/firebase/init";
import slugify from "slugify";
export default {
  data() {
    return {
      title: null,
      another: null,
      ingredients: [],
      feedback: null,
      slug: null,
    };
  },
  methods: {
    addKeep() {
      if (this.title) {
        this.feedback = null;
        // Create slug
        this.slug = slugify(this.title, {
          replacement: "-",
          remove: /[$*_+~.()'"!\-:@]/g,
          lower: true,
        });
        db.collection("keeps")
          .add({
            title: this.title,
            ingredients: this.ingredients,
            slug: this.slug,
          })
          .then(() => {
            this.$router.push({ name: "Home" });
          })
          .catch((err) => {
            console.log(err);
          });
      } else {
        this.feedback = "You must enter a keep title !";
      }
    },
    addItem() {
      if (this.another) {
        this.ingredients.push(this.another);
        this.another = null;
        this.feedback = null;
      } else {
        this.feedback = "You must enter a value to add an item !";
      }
    },
    deleteItem(ing) {
      this.ingredients = this.ingredients.filter((ingredient) => {
        return ingredient != ing;
      });
    },
  },
};
</script>

<style>
.add-keep {
  margin-top: 60px;
  padding: 20px;
}
.add-keep h2 {
  font-size: 2em;
  margin: 20px auto;
}
.add-keep .field {
  margin: 20px auto;
  position: relative;
}
.add-keep .delete {
  position: absolute;
  right: 0px;
  bottom: 16px;
  color: #aaa;
  font-size: 1.4em;
  cursor: pointer;
}
</style>
