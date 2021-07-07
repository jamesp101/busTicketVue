
<template>
  <main class="container p-0 md:p-5 lg:p-6 w-full h-full">
    <div v-if=isLoading
      class="w-full h-full">
      <div class="grid place-content-center w-full h-full">
        <Spinner class="text-7xl "></Spinner>
      </div>
    </div>

    <div v-else-if="isEmpty && !isLoading" class="grid place-content-center text-center h-full w-full">

      <font-awesome-icon :icon=faExclamation
        class="text-2xl text-gray-200 h-full"/>

      <h1 class="text-gray-500 mt-20">You have no tickets recorded.</h1>
      <nuxt-link to="/app/purchase" class="text-red-400">Purchase tickets here</nuxt-link>
    </div>

    <div v-else class="flex flex-col lg:flex-row h-full">

      <div class="h-full w-full">
        <div class="grid h-full w-full place-content-center">
          <div>

            <QrCode renderAs="canvas" size=200 class="w-full" :value="itemSelected.id.toString()" level="H"></QrCode>
          </div>

          <h1 class="mt-4 md:mt-14">Present to the conductor</h1>
          <h1 class="text-gray-600">Ticket ID: {{ itemSelected.id }} </h1>
          <h1>Status: {{ itemSelected.status }}</h1>


          <div class="mt-4 text-left font-light">
            <h1>Route: {{ itemSelected.route["description"] }}</h1>
            <h1>Quantity: {{ itemSelected.quantity  }}</h1>
            <h1>Date Purchased: {{ momentDate(itemSelected.created_at) }} </h1>
          </div>
        </div>

      </div>

      <div class="h-full w-full lg:w-3/12 ">
        <h1>Your Tickets</h1>

        <ul class="overflow-auto ">
          <li v-for="(item, index) in ticketList" :key="index"
            class=" font-light text-sm shadow-lg rounded-lg mx-1 "
          >

            <button class="w-full h-full text-left p-4 px-5 rounded-lg"
              v-on:click="selectItem(index)"
              v-bind:class="{
                      'border-solid border-l-8 border-red-400 font-bold'
                      : index == itemSelectIndex
                      }"
            >
              <h1>{{item.id}}</h1>
              <h1>{{ momentDate(item.created_at) }} </h1>
              <h1> {{item.route.description}}</h1>
            </button>

          </li>
        </ul>
      </div>
    </div>

  </main>

</template>

<script>
  import QrCode from 'qrcode.vue'
  import { faExclamation } from '@fortawesome/free-solid-svg-icons'
  import moment from 'moment'

  import Spinner from '~/components/Spinner'

  export default {


    layout: 'app_layout',
    components: {
      QrCode,
      Spinner
    },
    computed: {
      faExclamation(){return faExclamation},

    },

    async created () {

      let x =  await this.fetchTicketList()

      this.isLoading = false
      if (x.length != 0){
        this.isEmpty = false;
      }

      this.ticketList = x
      this.initialList = x

      this.selectItem(0)
    },

    methods : {
      async fetchTicketList() {
        let x = await this.$strapi.find('tickets', {
          user: this.$strapi.user.id
        })
        return x
      },

      async selectItem(index){
        this.itemSelectIndex = index

        this.itemSelected = this.initialList[index];
        console.log(this.initialList)

        this.itemSelected.id = this.itemSelected.id + ''
      },

      momentDate(date) {return moment(date).fromNow()},

      emitCredit() {
        this.$parent.$emit('updateCredit', true);

      }



    },


    data (){
      return{
        isEmpty: true,
        isLoading: true,


        initialList: [],

        ticketList: [],
        itemSelected: {},

        itemSelectIndex: 0
      }
    }
  }

</script>


<style scoped>
  svg {
    width: 200px !important;
    height: 200px !important;
  }
</style>
