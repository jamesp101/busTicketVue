<template>
  <div class="container h-full w-full mx-auto">
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
            :disabled=isFormValid
            v-bind:class="isFormValid ? 'bg-red-400 hover:bg-red-300' : 'bg-gray-200'"
          > Finish</button>
        </div>


      </div>
    </div>
  </div>
</template>

<script>
  import GeoMap  from '~/components/GeoMap'
  import { faMapMarkerAlt} from '@fortawesome/free-solid-svg-icons'

  export default {
    layout: 'app_layout',


    created() {
      this.fetchRoutes()
    },
    computed : {
      faMapMarkerAlt () {return faMapMarkerAlt},
      isFormValid(){return this.valid}
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


        if (res.length == 0){
          this.total = "Route is not available"
          this.valid = false
          return
        }
        this.total = "P " + (parseFloat(res[0].price) * parseFloat(this.passengers)).toFixed(2)
        this.valid=true
      },


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

        valid: false,

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
