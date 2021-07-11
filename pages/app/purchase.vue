<template>
  <div class="container h-full w-full mx-auto overflow-auto">
    <div class="h-full w-full flex flex-col md:flex-row">

      <!-- map -->
      <div class="h-1/2 w-full">
          <vl-map :load-tiles-while-animating="true" :load-tiles-while-interacting="true"
                    data-projection="EPSG:4326" class="w-full h-full">
              <vl-view :zoom.sync="zoom" :center.sync="center" :rotation.sync="rotation"></vl-view>

              <vl-layer-tile id="osm">
                <vl-source-osm></vl-source-osm>
              </vl-layer-tile>


              <template v-if="geoFrom.coordinatesArray">
                <vl-overlay :position=geoFrom.coordinatesArray >
                  <!-- <template slot-scope="scope">
                       <div class="overlay-content bg-red-400">
                       Hello world!<br>
                       Position: {{ scope.coordinates }}
                       </div>
                       </template> -->
                  <div class="overlay-content -translate-y-full">

                      <font-awesome-icon :icon=faMapMarkerAlt
                      class="text-2xl text-white h-full mx-auto" />
                      <h1 class="font-bold filter drop-shadow-lg">
                        From: {{geoFrom.description}}
                      </h1>

                  </div>
                </vl-overlay>
              </template>

              <template v-if="geoTo.coordinatesArray">
                <vl-overlay :position=geoTo.coordinatesArray >
                  <!-- <template slot-scope="scope">
                       <div class="overlay-content bg-red-400">
                       Hello world!<br>
                       Position: {{ scope.coordinates }}
                       </div>
                       </template> -->
                  <div class="overlay-content">
                      <font-awesome-icon :icon=faMapMarkerAlt
                      class="text-2xl text-red-600 h-full" />
                      <h1 class="font-bold filter drop-shadow-lg">
                        To: {{geoTo.description}}
                      </h1>
                  </div>
                </vl-overlay>
              </template>


            </vl-map>

      </div>
      <!-- map end -->

      <div class=" h-full w-full rounded shadow-lg ">
        <h1 class="text-center px-5 py-10 font-bold text-2xl">Purchase Ticket</h1>
        <div class="grid grid-rows-2 grid-cols-2 px-5">
          <label for="">
            From
          </label>
          <select id="" name="from" class="rounded-lg" v-model=locationQuery.from @change=getTotal >
            <option value="">--- Select ---</option>

            <option  v-for="item in locationList" :key="item.id" :value=item.id>
              {{ item.description  }}
            </option>
          </select>

          <label for="">To</label>
          <select id="" v-model=locationQuery.to name="to" class="rounded-lg" @change=getTotal >
            <option value="">--- Select ---</option>

            <option v-for="item in locationList" :key="item.id" :value="item.id">
              {{item.description}}
            </option>
          </select>
        </div>

        <div class="w-full px-5 my-8">
          <label for="">
            <p>Passengers:</p>
            <input type="number" name="" v-model=passengers class="rounded-lg" maxlength="3" max="100" min="1" @change=getTotal  />
          </label>
        </div>

        <div class="w-full my-4 px-5">
          <label for="">Aircon:</label>
          <input type="checkbox" name="" v-model=locationQuery.aircon class="p-2" @change=getTotal />
        </div>



        <hr class="my-4"/>



        <div class="flex flex-col px-5">

          <p class="font-bold text-lg w-full text-gray-400">Your Total</p>
          <p class="w-full py-4 font-bold text-4xl text-gray-600 antialiased">{{ total }}</p>

          <button class="font-bold rounded-lg w-full h-full p-4 text-white "
            v-bind:class="valid ? 'bg-red-400 hover:bg-red-300' : 'bg-gray-200'"
            @click=purchaseTicket
          > Finish</button>
        </div>


      </div>
    </div>
  </div>
</template>

<script>
  import GeoMap  from '~/components/GeoMap'
  import { faMapMarkerAlt} from '@fortawesome/free-solid-svg-icons'
  import Swal from 'sweetalert2'

  export default {
    layout: 'app_layout',


    created() {
      this.fetchRoutes()
    },
    computed : {
      faMapMarkerAlt () {return faMapMarkerAlt},
    },

    components : {
      GeoMap
    },

    methods: {
      async fetchRoutes () {

        let res = await this.$strapi.find('locations',)

        this.locationList = res
      },


      async getTotal(){
        const query = {
          'from.id': this.locationQuery.from,
          'to.id': this.locationQuery.to,
          'aircon': this.locationQuery.aircon
        }

        this.locationList.forEach( it=>{
          if (this.locationQuery.from == it['id']){
            this.geoFrom = it
          }
          if (this.locationQuery.to == it['id']){
            this.geoTo = it
          }
        })
        console.log(this.geoTo)


        try{
          this.geoFrom.coordinatesArray = this.geoFrom.coordinates.split(',').reverse()
          this.geoFrom.coordinatesArray[0] = parseFloat(this.geoFrom.coordinatesArray[0]);
          this.geoFrom.coordinatesArray[1] = parseFloat(this.geoFrom.coordinatesArray[1]);
        }
        catch{
        }
        try{
          this.geoTo.coordinatesArray = this.geoTo.coordinates.split(',').reverse()
          this.geoTo.coordinatesArray[0] = parseFloat(this.geoTo.coordinatesArray[0]);
          this.geoTo.coordinatesArray[1] = parseFloat(this.geoTo.coordinatesArray[1]);

        }catch{

        }


        let res = await this.$strapi.find('routes', query)
        console.log(res)

        if (res.length == 0){
          this.total = "Route is not available"
          this.valid = false
          return
        }
        this.totalFloat = (parseFloat(res[0].price) * parseFloat(this.passengers)).toFixed(2)
        this.total = "P " + this.totalFloat
        this.valid=true
        this.routeId = res[0].id

      },

      emitCredit() {
        this.$parent.$emit('updateCredit', true);
      },
      async purchaseTicket(){
        if (!this.valid ){
          return
        }

        let calculateDiff =  this.$strapi.user.current_coins - this.totalFloat

        if (calculateDiff <= 0){
          await Swal.fire({
            icon: 'error',
            text: 'You have insufficient coins. \n Purchase more coins'
          })
          return
        }

        let confirm = await Swal.fire({
          text: 'Confirm purchase?',
          showCancelButton: true,
          icon: 'question'
        })

        if (!confirm.isConfirmed) {
          return
        }

        let ticket = {
          user: this.$strapi.user.id,
          route: this.routeId,
          status: 'Available',
          quantity: this.passengers,
        }
        await this.$strapi.create('tickets', ticket)
        await this.$strapi.update('users', this.$strapi.user.id, {
          current_coins : calculateDiff
        })
        await Swal.fire ({
          title: 'Success',
          text: 'Successfully Purchased!',
          icon: 'success',
          confirmButtonText:'Ok',
          confirmButtonColor:'bg-red-400'
        })
        this.emitCredit()
        return


      }

    },


    data (){
      return {
        locationList: [],

        fromLocation:[],
        toLocation: [],

        geoFrom:{
        },
        geoTo:{},

        total: 'P 0.00',
        passengers: 1,
        totalFloat: 0,

        valid: false,

        routeId: '',

        locationQuery : {
          from: '',
          to: '',
          aircon: false,
        },

        zoom: 8.5,
        center: [125.18578460456244, 7.87327159352715],
        rotation: 0,
        geolocPosition: undefined,
      }


    }
  }
</script>

<style scoped>

</style>
