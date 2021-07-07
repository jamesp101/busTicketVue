<template>
  <main class="w-full h-screen
    flex justify-center items-center">

    <div class=" w-full md:w-5/12 lg:w-4/12
      border-solid border-2 p-5
      rounded-lg" >

      <img src="~/static/icon.jpg" alt=""
      class="h-auto w-6/12 mx-auto"/>

      <h1 class="font-black text-2xl text-center antialiased">Online Ticketing</h1>

      <label for="" >
        <p class="text-gray-500 font-light">Username or Email:</p>
        <input type="text" name="" value=""
          class="w-full rounded-lg"
          v-model=identifier
        />
      </label>

      <label for=""  class="mt-4">
        <p class="text-gray-500 font-light">Password:</p>
        <input type="password" name="" value=""
          class="w-full rounded-lg"
          v-model=password
        />
      </label>


      <button class="
        w-full
        font-bold  text-lg
        mt-5 p-4
        rounded-lg
        bg-red-400 text-white
        hover:bg-red-300"
        v-on:click=onLogin
      >
        Login
      </button>


      <div class="mt-5">
        <h1 class="text-gray-500">
          Don't have an account?
          <nuxt-link
            class="text-red-400"
            to="/register">
            Register
          </nuxt-link>,
        </h1>
      </div>

    </div>
  </main>
</template>



<script >
  export default {
    head: {
      title: "Login"
    },

    beforeMount() {
      this.rerouteUsers()

    },

    methods: {
      async onLogin(){
        try{
          await this.$strapi.login({
            identifier: this.identifier,
            password: this.password
          });
          this.rerouteUsers();
        }catch{
          alert("Username or password not found");
        }

      },
      rerouteUsers() {
        if (this.$strapi.user == null ){
          return;
        }
        console.log(this.$strapi.user)
        if(this.$strapi.user.role.id==3){
          this.$router.push('/conductor/')
          return
        }
        if(this.$strapi.user.role.id==1){
          this.$router.push('/app/')
          return
        }
      }
    },

    data() {
      return {
        identifier: '',
        password: ''
      }
    }
  }
</script>
