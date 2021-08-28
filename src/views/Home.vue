<template>
  <div class="home container">
    <div class="card" v-for="keep in keeps" :key="keep.id">
      <div class="card-content">
        <i class="material-icons delete" @click="deletekeep(keep.id)">delete</i>
        <h2 class="indigo-text">{{ keep.title }}</h2>
        <ul class="ingredients">
          <li v-for="(item, index) in keep.ingredients" :key="index">
            <span class="chip">{{ item }}</span>
          </li>
        </ul>
      </div>
      <span class="btn-floating btn-large halfway-fab pink">
        <router-link :to="{ name: 'Edit', params: { keep_slug: keep.slug } }">
          <i class="material-icons">edit</i>
        </router-link>
      </span>
    </div>
  </div>
</template>

<script>
import db from "@/firebase/init";

export default {
  name: "Home",
  components: {},
  data() {
    return {
      keeps: [],
    };
  },
  methods: {
    deletekeep(id) {
      db.collection("keeps")
        .doc(id)
        .delete()
        .then(() => {
          this.keeps = this.keeps.filter((keep) => {
            return keep.id != id;
          });
        });
    },
  },
  created() {
    db.collection("keeps")
      .get()
      .then((snapshot) => {
        snapshot.forEach((doc) => {
          let keep = doc.data();
          keep.id = doc.id;
          this.keeps.push(keep);
        });
      });
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

.home .delete {
  position: absolute;
  top: 4px;
  right: 4px;
  cursor: pointer;
  color: #aaa;
  font-size: 1.4em;
}
</style>
