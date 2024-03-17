<template>
  <div>
    <center>
      <h1>ADD NEW CATEGORY</h1>

      <br />
      <hr />
      <br />

      <div>
        <label>Type :- </label>
        <input type="text" name="type" v-model="type" />
      </div>
      <br />

      <button @click="onCreateCategory()">Save</button>

      <br /><br /><br />
      <hr />

      <span>Already Added Category List :-</span>
      <br><br><hr>
      <ul>
        <li v-for="cat in categoryList" :key="cat._id"> {{ cat.type }} </li>
      </ul>
    </center>
  </div>
</template>

<script>
export default {
  layout: "none",
  middleware: "auth",
  auth: "user",
  data() {
    return {
      type: "",
      categoryList: [],
    };
  },
  created : async function () {
    await this.onGetCategoryList();
  },
  methods: {
    onCreateCategory: async function () {
      try {
        if (this.type == "") {
          alert("Please Enter Category Name! ");
        } else {
          let responce = await this.$axios
            .$post("/api/category", {
              type: this.type,
            })
            .catch((e) => {
              alert("Problem With Connecting Server! ");
            });

          if (responce.success == true) {
            await this.onGetCategoryList();
          } else {
            alert(responce.message);
          }
        }
      } catch (error) {
        console.log("Eror While Create Category! ", error);
      }
    },

    onGetCategoryList: async function () {
      try {
        let responce = await this.$axios.$get("/api/category").catch((e) => {
          alert("Problem With Connecting Server!");
        });

        if (responce.success == true) {
            this.categoryList = responce.list;
          
        } 
      } catch (error) {
        console.log("Error While Category List ", error);
      }
    },
  },
};
</script>

<style>
</style>