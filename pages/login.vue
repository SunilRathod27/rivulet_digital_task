<template>
  <div>
    <center>
      <h1>LOGIN PAGE</h1>
      <hr />
      <br /><br />
      

      <div>
        <label>Email :- </label>
        <input type="email" name="email" v-model="email" />
      </div>
      <br />

      <div>
        <label>Password :- </label>
        <input type="password" name="password" v-model="password" />
      </div>
      <br /><br />

      <div>
        <button @click="onLoginUser()">Login</button>
      </div>

      <br /><br />
      <hr />

      <span
        >You Have Not An Account, Click Here
        <nuxt-link to="/signup"> <span>SignUP</span> </nuxt-link>
      </span>
    </center>
  </div>
</template>

<script>
export default {
  layout: "none",
  middleware : 'auth',
  auth : 'guest',
  data() {
    return {
      email: "",
      password: "",
    };
  },
  methods: {
    onLoginUser: async function () {
      try {
        if (!this.email || !this.password) {
          alert("All Field Is Required!");
        } else {
          let data = {
            email: this.email,
            password: this.password,
          };

          let responce = await this.$axios
            .$post("/api/auth/login", data)
            .catch((e) => {
              alert("Problem With Connecting Server!");
            });

          if (responce.success == true) {
            alert(responce.message);

            this.$auth.loginWith("local", {
              data: {
                email: this.email,
                password: this.password,
              },
            });

            this.$router.push('/');
    setTimeout(() => {
      window.location.reload();
    }, 400); 
          } else {
            alert(responce.message);
          }
        }
      } catch (error) {
        console.log("Error While Signp User ", error);
      }
    },
  },
};
</script>

<style>
</style>