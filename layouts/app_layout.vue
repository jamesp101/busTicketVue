<template>
  <div class="w-full h-screen bg-white overflow-hidden">

    <nav class="h-16 border-b-2 sticky top-0 z-50 bg-white">

      <button
      class="h-full p-4 b">

        <font-awesome-icon :icon=faBars
          class="transform duration-200 ease-in-out"
          v-bind:class="[ isNavShow ? 'rotate-90' : 'rotate-0'  ]"
          v-on:click=toggleNav
          />
      </button>

      <div class="float-right h-full  w-auto mr-0 md:mr-4
        grid place-content-center">
        <Coins class="my-auto"></Coins>
      </div>
    </nav>

    <div class="flex w-full h-full">
      <div class="w-60 h-full flex
        overflow-auto
        shadow-lg
        transform transition-opacity duration-700 ease-in-out"

        v-bind:class="[ isNavShow ? 'block' :  'hidden']"
      >
        <ul class="w-full p-5" >
          <li v-for="link in links" :key="link.to"
            class="w-full">
            <NuxtLink :to=link.to
              class="font-light text-gray-600 h-full w-full hover:font-bold" >
              <p class="p-4" >
                {{link.title}}
              </p>
            </NuxtLink>
          </li>
        </ul>
      </div>

      <div class="w-full h-full flex">
        <Nuxt></Nuxt>
      </div>
    </div>



  </div>
</template>


<script>

  import { faBars } from '@fortawesome/free-solid-svg-icons'
  import  Coins  from '~/components/Coins.vue'


  export default {
    computed : {
      faBars (){return faBars}
    },
    methods : {
      toggleNav() {
        this.isNavShow = !this.isNavShow
      }
    },
    components : {
      Coins

    },

    

    data() {
      return {
        isNavShow: true,

        links : [
          {
            to: '/app',
            title: 'Dashboard'
          },
          {
            to: '/app/ticket',
            title: 'Your tickets'
          },
          {
            to: '/app/purchase',
            title: 'Purchase Ticket'
          },
          {
            to: '/app/topup',
            title: 'Top-up'
          },
          {
            to: '/app/account',
            title: 'Account'
          },


        ]
      }

    }

  }

</script>


<style scoped>
  .nuxt-link-exact-active > p{
    border-left: solid #dc2626 0.2rem;
    font-weight: bold;
  }
</style>
