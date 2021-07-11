<template>
  <main class="container h-full mx-auto p-5">

    <div class="w-full h-20">
      <div class="flex flex-row gap-5">
          <button class="p-4 bg-white w-1/12 rounded-lg text-gray-500"
            @click=reload
          >
            Reload
          </button>

          <button class="p-4 bg-red-400 w-11/12 rounded-lg text-white"
            @click=manualInput
          >
            Manual
          </button>

        </div>
      <div class="">
        <QrcodeStream @decode="onDecode" @init=onInit v-if="!destroyed">
            <div class="loading-indicator" v-if="loading">
              Loading...
            </div>

        </QrcodeStream>
      </div>

    </div>

  </main>
</template>

<script>
  import Swal from 'sweetalert2'
  import { QrcodeStream, } from 'vue-qrcode-reader'

  export default {
    layout: 'conductor_layout',
    components: {
      QrcodeStream
    },
    methods :{
      onDecode (code){
        this.updateTicket(code)
      },
      async manualInput(){
        const { value: code } = await Swal.fire({
          title: 'Enter Ticket',
          input: 'text',
          inputLabel: 'Ticket ID:',
          showCancelButton: true,
        })
        this.updateTicket(code)
        this.reload()
      },
      async reload () {
        this.destroyed = true

        await this.$nextTick()

        this.destroyed = false
      },


      async onInit(promise){
        try {
        await promise
      } catch (error) {
        console.error(error)
      } finally {
        this.loading = false
      }
      },

      async updateTicket(code) {

       let  i = await this.$strapi.find('tickets', {'id': code} )

        i = i[0]

        if(i.length == 0){
          await Swal.fire({
            icon: 'error',
            text: 'Cannot find id'
          })
          this.reload()
          return
        }
        console.log(i)

        if(i.status == "Claimed"){
          await Swal.fire({
            icon: 'error',
            text: 'Ticket is already claimed'
          })
          this.reload()
          return
        }


        let o = await Swal.fire({
          icon: 'question',
          text: 'Confirm? Ticket ID:' + code,
          showCancelButton: true,
        })

        if (!o.isConfirmed) {

          this.reload()
          return
        }

        let l = await this.$strapi.update('tickets',this.$strapi.user.id,
                            {confirmedBy: this.$strapi.user.id,
                             status: 'Claimed'})

        let h = await Swal.fire({
          icon: 'success',
          text: 'Successfully claimed. Scan another one?',
          showCancelButton: true,
        })

        if (!h.isConfirmed) {
          this.reload()
          return
        }

        this.$router.push('/conductor')
      },

    },

    data (){
      return {

        track: 'bounding box',
        loading: true,
        destroyed:false

      }
    }
  }
</script>

<style scoped>

</style>
