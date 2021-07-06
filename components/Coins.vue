
<template>
  <div class="flex space-x-4 ">

    <font-awesome-icon :icon=faCoins
    class="text-2xl text-red-500 h-full"/>

    <h1 class="font-bold text-gray-400 h-full" v-if=isLoading>
      <font-awesome-icon :icon=faSpinner
      class="text-2xl text-gray-500 h-full animate-spin" />
    </h1>

    <h1 class="font-bold text-gray-400 h-full" v-else > {{myCoins}}</h1>


    <nuxt-link to="/app/topup" class="font-bold px-3 h-full"> + </nuxt-link>

  </div>

</template>

<script>
  import { faCoins } from '@fortawesome/free-solid-svg-icons'
  import { faSpinner } from '@fortawesome/free-solid-svg-icons'


  export default {
    computed : {
      faCoins (){return faCoins},
      faSpinner() {return faSpinner}
    },
    created(){
      this.fetchCoinsInfo();
    },
    methods :{
      async fetchCoinsInfo(){
        if (this.$strapi.user == null) {
          this.$router.push('/');
          return;
        }
        await this.$strapi.fetchUser();

        let user = this.$strapi.user
        this.myCoins = user.current_coins
        this.isLoading = false
      }
    },
    props :{
      coins: String

    },
    data () {
      return {
        myCoins: 0,
        isLoading: true
      }
    }
  }

</script>
