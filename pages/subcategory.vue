<template>
  <div>
    <center>
      <h1>CREATE NEW SUBCATEGORY</h1>
      <br /><br />
      <hr />

      <div>
        <label> Select Category :- </label>
        <select v-model="category">
          <option v-for="cat in categoryList" :key="cat._id" :value="cat._id">
            {{ cat.type }}
          </option>
        </select>
      </div>
      <br />

      <div>
        <label>SubCategory Type :- </label>
        <input type="text" name="type" v-model="type" />
      </div>
      <br /><br />

      <button @click="onAddSubCategory()">Save</button>
      <br /><br />
      <hr />

      <span>Already Added SubCategory List</span>

      <table>
        <tr>
          <th>Category</th>
          <th>Sub Category</th>
        </tr>
        <tr v-for="data in subCategoryList" :key="data._id">
          <td>{{ data.catg }}</td>
          <td>{{ data.type }}</td>
        </tr>
      </table>
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
      category: "",
      type: "",
      categoryList: [],
      subCategoryList: [],
    };
  },
  created: async function () {
    await this.onGetCategoryList();
    await this.onGetSubCategoryList();
  },
  methods: {
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

    onGetSubCategoryList: async function () {
      try {
        let responce = await this.$axios.$get("/api/subcategory").catch((e) => {
          alert("Problem With Connecting Server!");
        });

        if (responce.success == true) {
          this.subCategoryList = responce.list;
        }
      } catch (error) {
        console.log("Error While Sub Category List ", error);
      }
    },

    onAddSubCategory: async function () {
      try {
        if (!this.category || !this.type) {
          alert("All Field is Required!");
        } else {
          let data = {
            category: this.category,
            type: this.type,
          };

          let responce = await this.$axios
            .$post("/api/subcategory", data)
            .catch((e) => {
              alert("Problem With Connecting Server!");
            });

          if (responce.success == true) {
            this.category = '';
            this.type = '';
            alert(responce.message);
            await this.onGetSubCategoryList();
          } else {
            alert(responce.message);
          }
        }
      } catch (error) {
        console.log("Error While Add SubCategorey ", error);
      }
    },
  },
};
</script>

<style>
</style>