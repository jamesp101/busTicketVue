<template>
  <main class=" container mx-auto">

    <div class="w-full h-full p-4">
      <h1 class="font-bold text-2xl text-gray-700">Select: </h1>

      <div class="flex gap-2 h-full flex-wrap" >
        <button class="w-full md:w-6/12 lg:w-3/12
          text-left
          shadow-lg
          rounded-lg
          hover:bg-red-50
          p-4"
          @click="purchaseCredit(50)"
        >
          <div class="w-full flex font-bold">
            <font-awesome-icon :icon=faCoins
              class="text-2xl text-red-500 h-full"/>
            <h1 class="ml-4 text-lg text-gray-800">+50 </h1>
          </div>

          <div class="w-full flex font-light mt-3">

            <h1>P 49.99</h1>
          </div>

        </button>
        <button class="w-full md:w-6/12 lg:w-3/12
          text-left
          shadow-lg
          rounded-lg
          hover:bg-red-50
          p-4
          "
          @click="purchaseCredit(100)"
        >

          <div class="w-full flex font-bold">
            <font-awesome-icon :icon=faCoins
              class="text-2xl text-red-500 h-full"/>
            <h1 class="ml-4 text-lg text-gray-800">+100</h1>
          </div>

          <div class="w-full flex font-light mt-3">

            <h1>P 99.99</h1>
          </div>

        </button>

        <button class="w-full md:w-6/12 lg:w-3/12
          text-left
          shadow-lg
          rounded-lg
          hover:bg-red-50
          p-4 "
          @click="purchaseCredit(210)"
        >

          <div class="w-full flex font-bold">
            <font-awesome-icon :icon=faCoins
              class="text-2xl text-red-500 h-full"/>
            <h1 class="ml-4 text-lg text-gray-800">+210</h1>
          </div>

          <div class="w-full flex font-light mt-3">

            <h1>P 199.99</h1>
          </div>

        </button>

        <button class="w-full md:w-6/12 lg:w-3/12
          text-left
          shadow-lg
          rounded-lg
          hover:bg-red-50
          p-4"
          @click="purchaseCredit(525)"
        >

          <div class="w-full flex font-bold">
            <font-awesome-icon :icon=faCoins
              class="text-2xl text-red-500 h-full"/>
            <h1 class="ml-4 text-lg text-gray-800">+525</h1>
          </div>

          <div class="w-full flex font-light mt-3">

            <h1>P 500.00</h1>
          </div>

        </button>
      </div>
    </div>

  </main>
</template>

<script>

  import { faCoins } from '@fortawesome/free-solid-svg-icons'
  import Swal from 'sweetalert2'
  import valid from 'card-validator'

  export default {

    layout: 'app_layout',
    computed :{
      faCoins () { return faCoins }
    },
    mounted () {
      console.log(this.$strapi.user.current_coins)

    },
    mounted() {
      this.$strapi.hook('userUpdated', user => {
        console.log('hook:' ,user)
      })
    },

    methods: {
      emitCredit() {
        this.$parent.$emit('updateCredit', true);
      },


      async purchaseCredit(value){
        this.getMethod()
       let confirm = await  Swal.fire({
         title: 'Confirm',
         icon:'warning',
          text: 'Are you sure you want to purchase this?',
          showCancelButton: true,
        })

        if (!confirm.isConfirmed) {
          return
        }

        let updateval = {
          current_coins: this.$strapi.user.current_coins + value
        }

        if(!( await this.getMethod() )){
          return
        }

        await this.$strapi.update('users', this.$strapi.user.id, updateval)
        Swal.fire({
          title: 'Success',
          icon: 'success',
          text: 'You have successfully purchased credits'
        })
        //this.$strapi.user.current_coins = this.$strapi.usercurrent_coins + value

        this.emitCredit();

      },


      async getMethod(){
        let method = await Swal.fire({
          text: 'Select method',
          input: 'select',
          inputOptions: {
            master: 'Master',
            visa: 'Visa'
          },
          showCancelButton: true
        })

        if(method.value == 'master'){
          let m = await Swal.fire({
            text: 'Input MasterCard number',
            input: 'text',
            showCancelButton: true,
            inputValidator: value =>{

              if(!valid.number(value).isValid){
                return 'MasterCard is not valid'
              }
            }

          })
          return m.isConfirmed
        }
        if(method.value == 'visa'){
          let m = await Swal.fire({
            text: 'Input Visa number',
            input: 'text',
            showCancelButton: true,
            inputValidator: value =>{

              if(!valid.number(value).isValid){
                return 'Visa is not valid'
              }
            }
          })
            return m.isConfirmed
        }

      }

    }
  }
</script>

<style>

</style>
