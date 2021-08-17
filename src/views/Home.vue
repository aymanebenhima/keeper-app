<template>
  <div class="home container">
    <div class="card" v-for="keep in keeps" :key="keep.id">
      <div class="card-content">
        <h2 class="indigo-text">{{ keep.title }}</h2>
        <ul class="ingredients">
          <li v-for="(item, index) in keep.ingredients" :key="index">
            <span class="chip">{{ item }}</span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: "Home",
  components: {},
  data() {
    return {
      keeps: [
        {
          id: 1,
          title: "Aymane Benhima",
          slug: "aymane-benhima",
          ingredients: ["front-end", "back-end"],
        },
        {
          id: 2,
          title: "Ouail Benhima",
          slug: "ouail-benhima",
          ingredients: ["css", "js"],
        },
      ],
    };
  },
};
</script>

<style>
.home {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 30px;
  margin-top: 60px;
}
.home h2 {
  font-size: 1.8em;
  text-align: center;
  margin-top: 0;
}
.home .ingredients {
  margin: 30px auto;
}
.home .ingredients li {
  display: inline-block;
}
</style>
