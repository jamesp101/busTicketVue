
<template>
  <div class="w-full h-screen bg-white app-grid">
    <nav class="col-start-1 col-end-13
      bg-red-400
      shadow-lg
      sticky">

      <div class="float-left flex flex-row">
      <button
        class="h-full p-4 ">
        <font-awesome-icon :icon=faBars
          class="transform duration-200 ease-in-out"
          v-bind:class="[ isNavShow ? 'rotate-90' : 'rotate-0'  ]"
          v-on:click=toggleNav
        />
      </button>
      <div class="grid place-content-center">
        <h1 class="text-white subpixel-antialiased">PABAMA Conductor app</h1>
      </div>

      </div>


      <div class="float-right h-full  w-auto mr-0 md:mr-4
        grid place-content-center">
      </div>

    </nav>
    <div class="
       h-full flex
      overflow-auto
      shadow-lg
      transform transition-opacity duration-700 ease-in-out
      min-w-60
      absolute md:relative
      bg-white
      z-50
      "
      v-bind:class="[ isNavShow ? 'block' :  'hidden']"
    >
      <ul class="w-full p-5" >
        <li class="visible md:hidden">
          <button class="p-4">
            <font-awesome-icon :icon=faArrowLeft
              class="transform duration-200 ease-in-out"
              v-on:click=toggleNav
            />
          </button>
        </li>
        <li v-for="link in links" :key="link.to"
          class="w-full">
          <NuxtLink :to=link.to
            class="font-light text-gray-600 h-full w-full hover:font-bold" >
            <p class="p-4" >
              {{link.title}}
            </p>
          </NuxtLink>

        <li>
            <button class="p-4
              font-light text-gray-600 h-full w-full hover:font-bold
              text-left
              "
              @click=logout
            >
              Logout
            </button>
        </li>
      </ul>
    </div>
    <div class="col-start-1 md:col-start-2 col-end-13">
      <Nuxt @updateCredit="onUpdateCredit"></Nuxt>
    </div>



  </div>
</template>


<script>

  import { faBars, faArrowLeft } from '@fortawesome/free-solid-svg-icons'
  import Swal from 'sweetalert2'



  export default {
    computed : {
      faBars (){return faBars},
      faArrowLeft () {return faArrowLeft}
    },

    beforeCreate() {
      if (this.$strapi.user == null){
        this.$router.push('/')
      }
    },


    methods : {
      toggleNav() {
        this.isNavShow = !this.isNavShow
      },
      async logout () {

        let confirm = await Swal.fire({
          title: '',
          text: 'Logout?',
          icon:'question',
          showCancelButton: true
        });

        if (!confirm.isConfirmed) {
          return
        }

        this.$strapi.logout()
        this.$router.push('/')
      },
      onUpdateCredit() {
        console.log('requesting update on credit')
      }
    },




    components : {

    },



    data() {
      return {
        isNavShow: true,

        links : [
          {
            to: '/conductor',
            title: 'Dashboard'
          },
          {
            to: '/conductor/scan',
            title: 'Scan'
          },
          {
            to: '/conductor/history',
            title: 'History'
          },
          {
            to: '/conductor/account',
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
  .app-grid{
    display: grid;
    grid-template-rows: 4rem auto;

  }
</style>
