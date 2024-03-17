<template>
  <div>
    <center>
      <h1>{{ pageTitle }}</h1>
      <br />
      <hr />
      <div>
        <label>Select Category :-</label>
        <select v-model="category">
          <option
            v-for="catg in categoryList"
            :key="catg._id"
            :value="catg._id"
          >
            {{ catg.type }}
          </option>
        </select>
      </div>
      <br />
      <div>
        <label> Name :- </label>
        <input type="text" name="name" v-model="name" />
      </div>
      <br />
      <div>
        <label> Description :- </label>
        <input type="text" name="description" v-model="description" />
      </div>
      <br />
      <div>
        <label> Price :- </label>
        <input type="number" name="price" v-model="price" />
      </div>
      <br />
      <div>
        <label> StockQty :- </label>
        <input type="number" name="stockQty" v-model="stockQty" />
      </div>
      <br />
      <div>
        <label>Add Photo</label>
        <div
          class="imagewrap"
          :style="{ 'background-image': `url(${previewImg})` }"
          @click="selectImage()"
        ></div>
        <input
          type="file"
          style="display: none"
          ref="fileInput"
          @input="onFileSelected($event)"
        />
      </div>
      <br /><br />
      <hr />
      <div>
        <button @click="onCreateandUpdateProduct()">Save</button>
      </div>
    </center>
  </div>
</template>

<script>
export default {
  middleware: "auth",
  auth: "user",
  layout: "none",
  data() {
    return {
      productID: "",
      pageTitle: "",
      previewImg:
        "https://upload.wikimedia.org/wikipedia/commons/thumb/6/65/No-Image-Placeholder.svg/1665px-No-Image-Placeholder.svg.png",
      categoryList: [],
      baseURL: this.$axios.defaults.baseURL,
      category: "",
      name: "",
      description: "",
      price: "",
      stockQty: "",
      selectedFile: null,
      oldImg: "",
    };
  },
  created: async function () {
    this.productID = this.$route.params.id;
    if (this.productID) {
      this.pageTitle = "UPDATE PRODUCT";
      await this.getSingleProduct(this.productID);
    } else {
      this.pageTitle = "ADD PRODUCT";
    }
    await this.onGetCategoryList();
  },
  methods: {

    selectImage: async function () {
      try {
        this.$refs.fileInput.click();
      } catch (error) {
        console.log("selectImageselectImage", error);
      }
    },

    onFileSelected: async function (event) {
      try {
        let input = this.$refs.fileInput;
        let file = input.files;
        if (file && file[0]) {
          let reader = new FileReader();
          reader.onload = (e) => {
            this.previewImg = e.target.result;
          };
          reader.readAsDataURL(file[0]);
          this.$emit("input", file[0]);
          this.selectedFile = event.target.files[0];
        }
      } catch (error) {
        console.log("onFileSelectedonFileSelected", error);
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

    onCreateandUpdateProduct: async function () {
      try {
        let data = new FormData();
        let result;
        data.append("category", this.category);
        data.append("name", this.name);
        data.append("description", this.description);
        data.append("price", this.price);
        data.append("stockQty", this.stockQty);
        data.append("oldImg", this.oldImg);

        if (this.selectedFile) {
          data.append("photo", this.selectedFile, this.selectedFile.name);
        }

        if (this.productID) {
          result = await this.$axios
            .$patch(`/api/product/${this.productID}`, data)
            .catch((e) => {
              alert("Problem With Connecting Server!");
            });
        } else {
          result = await this.$axios.$post("/api/product", data).catch((e) => {
            alert("Problem With Connecting Server!");
          });
        }

        if (result.success == true) {
          alert(result.message);
          this.$router.push("/");
        } else {
          alert(result.message);
        }
      } catch (error) {
        console.log("onCreateandUpdateProduct", error);
      }
    },

    getSingleProduct : async function (id) {
      try {
          let result = await this.$axios.$get(`/api/product/${id}`).catch((e) => {
            alert("Problem With Connecting Server!");
          });

          if (result.success == true) {
              let data = result.product;
              this.category = data.category;
              this.name = data.name;
              this.description = data.description;
              this.price = data.price;
              this.stockQty = data.stockQty;
              this.previewImg = this.baseURL + data.photo;
              this.oldImg = data.photo;
              alert(result.message)
          }
          else {
            alert(result.message)
          }
      } catch (error) {
        console.log("getSingleProduct", error);
      }
    }
  },
};
</script>

<style scoped>
.imagewrap {
  width: 100px;
  height: 100px;
  display: block;
  cursor: pointer;
  background-size: cover;
  background-position: center center;
}
</style>