<template>
  <div v-if="keep" class="edit-keep container">
    <h2>Edit {{ keep.title }} keep</h2>
    <form @submit.prevent="editKeep">
      <div class="field title">
        <label for="title">Keep Title:</label>
        <input type="text" name="title" v-model="keep.title" />
      </div>
      <div class="field" v-for="(ing, index) in keep.ingredients" :key="index">
        <label for="item">Item: {{ index + 1 }}</label>
        <input type="text" name="item" v-model="keep.ingredients[index]" />
        <i class="material-icons delete" @click="deleteItem(ing)">delete</i>
      </div>
      <div class="filed edit-keep">
        <label for="edit-keep">edit keep and (press SPACE):</label>
        <input
          @keydown.space.prevent="editItem"
          v-model="another"
          type="text"
          name="edit-keep"
        />
      </div>
      <div class="field center-align">
        <p v-if="feedback" class="red-text">{{ feedback }}</p>
        <button class="btn pink">Update</button>
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
      keep: null,
      another: null,
      feedback: null,
    };
  },
  created() {
    let ref = db
      .collection("keeps")
      .where("slug", "==", this.$route.params.keep_slug);
    ref.get().then((snapshot) => {
      snapshot.forEach((doc) => {
        this.keep = doc.data();
        this.keep.id = doc.id;
      });
    });
  },
  methods: {
    editKeep() {
      if (this.keep.title) {
        this.feedback = null;
        // Create slug
        this.keep.slug = slugify(this.keep.title, {
          replacement: "-",
          remove: /[$*_+~.()'"!\-:@]/g,
          lower: true,
        });
        db.collection("keeps")
          .doc(this.keep.id)
          .update({
            title: this.keep.title,
            ingredients: this.keep.ingredients,
            slug: this.keep.slug,
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
    editItem() {
      if (this.another) {
        this.keep.ingredients.push(this.another);
        this.another = null;
        this.feedback = null;
      } else {
        this.feedback = "You must enter a value to add an item !";
      }
    },
    deleteItem(ing) {
      this.keep.ingredients = this.keep.ingredients.filter((ingredient) => {
        return ingredient != ing;
      });
    },
  },
};
</script>

<style>
.edit-keep {
  margin-top: 60px;
  padding: 20px;
}
.edit-keep h2 {
  font-size: 2em;
  margin: 20px auto;
}
.edit-keep .field {
  margin: 20px auto;
  position: relative;
}
.edit-keep .delete {
  position: absolute;
  right: 0px;
  bottom: 16px;
  color: #aaa;
  font-size: 1.4em;
  cursor: pointer;
}
</style>
