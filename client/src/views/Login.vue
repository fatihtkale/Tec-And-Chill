<template>
  <form class="form" v-on:submit.prevent="onSubmit">
    <div class="login-body">
      <div class="login-icon">TC</div>
      <p class="text">Brugernavn</p>
      <input class="input-field" name="username" type="text">
      <p class="text">Adgangskode</p>
      <input class="input-field" name="password" type="password">
      <button class="button" type="submit">Login</button>
      <div class="login-footer">
        <router-link to="/register">Ny Bruger?</router-link>
        <router-link to="#">Glemt Adgangskode?</router-link>
      </div>
    </div>
  </form>
</template>

<script>
import axios from 'axios';
import { useStore } from 'vuex'
import { useRouter } from 'vue-router'
import { useToast } from "vue-toastification";

export default {
  setup(){
    const router = useRouter()
    const toast = useToast();
    var state = useStore();

    function onSubmit(e){
      var form = e.target;
      axios.post("http://localhost:3000/login", {
        username: form.username.value,
        password: form.password.value,
      }).then(response => {
        if (response.data.status == "OK") {
          state.commit("UPDATE_JWT", response.data.token);
          state.commit("UPDATE_LOGGED_IN", true);
          console.log(response);
          toast.success("Logged in!")
          router.push("/")
        } else {
          toast.error(response.data.status)
        }
      }).catch(error => {
        console.log(error)
      })
    }

    return { onSubmit }
  }
}
</script>

<style scoped>

.form{
  top: 50%;
  left: 50%;
  transform: translate3d(-50%,-50%, 0);
  position: absolute;
  background-color: #202020;
  border: 2px solid #303030;
  border-radius: 8px;
  padding: 25px 44px;
}
.login-body{
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
  color: white;
}
.login-icon{
  text-align: center;
  font-size: 48px;
  font-weight: bold;
  color: #51B973;
}
.input-field{
  padding: 10px;
  font-size: 20px;
  color: white;
  background-color: #3F3F3F;
  border: 2px solid #707070;
  border-radius: 5px;
  outline: none;
  width: 100%;
  margin-right: 80px;
  box-sizing: border-box;
}
.text{
  font-size: 20px;
  font-weight: 400;
  margin-top: 30px;
}
.button{
  width: 100%;
  background-color: #59CB7F;
  border: 2px solid #51B973;
  border-radius: 5px;
  padding: 13px 0;
  color: white;
  font-size: 20px;
  margin-top: 60px;
}
.login-footer {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-between;
  align-content: flex-start;
  align-items: space-between;
}
.login-footer a{
  font-size: 20px;
  text-decoration: none;
  color: #3F3F3F;
  margin-top: 60px;
  margin-bottom: 11px;
}
/* Smartphones (portrait and landscape) ----------- */
@media only screen and (max-width : 425px){
  .form{
    padding-right: 10px;
    padding-left: 10px;
  }
}
/* iPads (portrait and landscape) ----------- */
@media only screen and (max-width : 768px){
  .form{
    width: 100%;
    border-radius: 0 !important;
  }
  .input-field{
    margin-right: 0;
  }
}
</style>