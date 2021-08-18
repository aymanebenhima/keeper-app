<template>
  <div class="add-keep container">
    <h2 class="center-align indigo-text">Add new keep</h2>
    <form @submit.prevent="addKeep">
      <div class="field add-keep">
        <label for="title">Keep Title</label>
        <input type="text" name="title" v-model="title" />
      </div>
      <div class="add-keep" v-for="(item, index) in items" :key="index">
        <label for="item">Item: {{ index + 1 }}</label>
        <input type="text" name="item" v-model="items[index]">
      </div>
      <div class="filed add-keep">
        <label for="add-keep">Add keep and (press SPACE):</label>
        <input @keydown.space.prevent="addItem" v-model="another" type="text" name="add-keep" />
      </div>
      <div class="field center-align">
        <p v-if="feedback" class="red-text">{{ feedback }}</p>
        <button class="btn pink">Add keep</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: null,
      another: null,
      items: [],
      feedback: null,
    };
  },
  methods: {
    addKeep() {
      console.log(this.title);
    },
    addItem() {
      if (this.another) {
        this.items.push(this.another);
        this.another = null;
        this.feedback = null;
      } else {
        this.feedback = "You must enter a value to add an item !";
      }
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
}
</style>
