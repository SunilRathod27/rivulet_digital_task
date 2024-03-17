<template>
  <div>
    <center>
      <h1>SIGNUP PAGE</h1>
      <hr />
      <br /><br />

      <div>
        <label>Name :- </label>
        <input type="text" name="name" v-model="name" />
      </div>
      <br />

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
        <button @click="onSignUpUser()">SignUp</button>
      </div>

      <br /><br />
      <hr />

      <span
        >You Have Already Account, Click Here
        <nuxt-link to="/login"> <span>Login</span> </nuxt-link>
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
      name: "",
      email: "",
      password: "",
    };
  },
  methods: {
    onSignUpUser: async function () {
      try {
        if (!this.name || !this.email || !this.password) {
          alert("All Field Is Required!");
        } else {
          let data = {
            name: this.name,
            email: this.email,
            password: this.password,
          };

          let responce = await this.$axios
            .$post("/api/auth/signin", data)
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

            this.$router.push("/");
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