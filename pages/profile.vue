<template>
  <div>
    <center>
    <h1>PROFILE PAGE :- {{ $auth.$state.user.name }} </h1>
    <br><hr><br>

    <button @click="onLogOutUser()">LogOut</button>

    <br><hr><br>

    <div>
        <label >Name :- </label>
        <input type="text" name="name" :placeholder="$auth.$state.user.name">
    </div>
    <br>

    <div>
        <label >Email :- </label>
        <input type="email" name="email" :placeholder="$auth.$state.user.email">
    </div>
    <br>

    <div>
        <label >Password :- </label>
        <input type="password" name="password" placeholder="*******************">
    </div>
    <br>

    </center>
  </div>
</template>

<script>
export default {
    middleware : 'auth',
    auth : 'user',
    methods : {
        onLogOutUser : async function () {
            try {
                await this.$auth.logout();
				 window.location.reload(true);
               	await this.$router.push('/login');

            } catch (error) {
                console.log("Error While Get Logout User :- ", error);
            }
        }
    }
}
</script>

<style>

</style>