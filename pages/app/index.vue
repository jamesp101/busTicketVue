
<template>
  <main class="container mx-auto">
    <div class="w-full h-full py-5" >

      <div class="flex  w-full gap-2 justify-center">
      <button to="/app/ticket" class="w-full md:w-5/12 lg:w-4/12
        bg-gradient-to-r from-purple-400 to-red-500
        rounded-lg p-5"
        @click=clickUnusedTicket
      >
          <h1 class="text-white font-black w-full text-left text-5xl">
            {{tickets}} ticket/s
          </h1>

          <h1 class="text-white text-left" v-if="tickets!=0"> Available</h1>
          <h1 class="text-white text-left font-light" v-else> Purchase tickets here </h1>
          <h1 class="text-white text-left font-light">Click to use tickets</h1>
      </button>

      <button to="/app/ticket" class="w-full md:w-5/12 lg:w-4/12
        bg-gradient-to-r from-purple-400 to-red-500
        rounded-lg p-5"
        @click=clickCoins
      >
        <h1 class="text-white font-black w-full text-left text-5xl">
          {{credit}} Coins
          </h1>
          <h1 class="text-white text-left font-light">Available</h1>
          <h1 class="text-white text-left font-light">Purchase more!</h1>
      </button>

      </div>
    </div>
  </main>
</template>

<script >

export default {
    layout: 'app_layout',


    created(){
      this.getUnusedTicket()
      this.getCredit()

      console.log(this.$strapi.user)
    },

    methods: {

      clickCoins(){
        this.$router.push('/app/topup')
      },
      clickUnusedTicket(){

        if(this.tickets!=0){
          this.$router.push('/app/ticket')
        }else{
          this.$router.push('/app/purchase')
        }
      },
      async getUnusedTicket(){
        let x = await this.$strapi.count('tickets', {
          user: this.$strapi.user.id,
          status: 'Available'
        })
        this.tickets = x

      },
      async getCredit(){
        this.credit = this.$strapi.user.current_coins
      },
    },

    data() {
      return {
        tickets: 0,
        credit: 0
      }

    }

  }


</script>
