<template>
  <div>
    <center>
        <h1>SHOPPING CART</h1>
    </center>
    <div class="main">
        <div class="cartdiv" v-for="product in getCart" :key="product._id"> 
            <div>
                <img :src="baseURL + product.photo" alt="product" height="50" width="50">
            </div>
            <div>
                <span> ₹ {{ product.price * product.quantity }} </span>
            </div>
            <div>
                <span> {{ product.name }} </span>
            </div>
            <div>
                <button @click="$store.commit('removeProduct', product)">Delete</button>
            </div>
        </div>
    </div>
    <div>
      <h3>
      SubTotal ({{ getCartLength }} Item): <b>₹ {{ totalPriceWithDelivery }}</b>
      <span v-if="deliveryFeeApplied"> + ₹50 Delivery Fee</span>
	  <span v-else> + ₹0 Delivery Fee</span>
    </h3>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
export default {
    middleware : 'auth',
    auth : 'user',
    data() {
        return {
            baseURL : this.$axios.defaults.baseURL,
        }
    },
    computed : {
		...mapGetters(["getCart", "getCartTotalPrice", "getCartLength"]),
		totalPriceWithDelivery() {
      let totalPrice = this.getCartTotalPrice;
      // Check if the total price is greater than 500
      if (totalPrice > 500) {
        // Add delivery fee of 50
        totalPrice += 50;
      }
      return totalPrice;
    },
	deliveryFeeApplied() {
      return this.getCartTotalPrice > 500;
    }
 
		 
    }
}
</script>

<style scoped>
.main {
    height: 300px;
    width: 800px;
    overflow: auto;
}
.cartdiv {
    height: 150px;
    width: 780px;
    padding: 10px;
    margin-bottom: 10px;
}
</style>