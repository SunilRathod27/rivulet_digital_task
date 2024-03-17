<template>
  <div>
    <center>
		
      <h4>Product List</h4>
	  <input type="text" v-model="searchTerm" placeholder="Search by name" @input="onGetProductList">
    </center>
    <hr />
    <div class="main">
		

      <div class="box" v-for="product in productList" :key="product._id">
       <div>
  <img :src="baseURL + product.photo" alt="product" width="100" height="100" />
</div>

        <div>
          <center>
            <p>
              <b> ₹{{ product.price }} </b>
            </p>
            <p>
              <b>{{ product.name }}</b>
            </p>
            <p>{{ product.description }}</p>
          </center>
        </div>
        <div class="probtn">
          <button>
            <nuxt-link :to="`/product/${product._id}`"
              ><span>Update</span></nuxt-link
            >
          </button>
          <button @click="onDeleteProduct(product)">Delete</button>
          <button @click="addProductToCart(product)"> AddToCart</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapActions } from "vuex";
export default {
  data() {
    return {
      baseURL: this.$axios.defaults.baseURL,
      productList: [],
	  searchTerm :''
    };
  },
  created: async function () {
    await this.onGetProductList();
  },
  methods: {
    ...mapActions(["addProductToCart"]),
    
    onGetProductList: async function () {
      try {
        let searchTerm = this.searchTerm; 
	    let response;
    if (searchTerm) {
      response = await this.$axios.$post("/api/product", { search: searchTerm }).catch((e) => {
        alert("Problem With Connecting Server");
      });
    } else {
      response = await this.$axios.$post("/api/product").catch((e) => {
        alert("Problem With Connecting Server");
      });
    }

    if (response.success == true) {
      this.productList = response.list;
    }
      } catch (error) {
        console.log("Error While Get Product List ", error);
      }
    },
	
    onDeleteProduct : async function (data) {
      try {

        let deleteProd = false;
        let text = `Are You Sure You Want To Delete This Product:- ${data.name}`
        if (confirm(text) == true) {
          deleteProd = true
        }
        else {
          deleteProd = false
          alert('User Cancle Delete Product!')
        }

        if (deleteProd) {
            let result = await this.$axios.$delete(`/api/product/${data._id}`).catch((e)=> {
              alert('Problem With Connecting Server!');
            });

            if (result.success == true) {
                alert(result.message);
                await this.onGetProductList();
            }
            else {
                alert(result.message);
            }
        }
        

        
      } catch (error) {
        console.log("onDeleteProduct", error);
      }
    }
  },
};
</script>

<style scoped>
.main {
  display: grid;
  grid-template-columns: auto auto auto auto;
}
.box {
  height: 400px;
  width: 300px;
  border: 1px solid black;
  padding: 10px;
  margin-left: 15px;
}
.probtn {
  display: flex;
  align-items: center;
  justify-content: space-around;
}
</style>