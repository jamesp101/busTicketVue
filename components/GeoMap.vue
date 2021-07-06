<template>
  <div>
    <vl-map :load-tiles-while-animating="true" :load-tiles-while-interacting="true"
             data-projection="EPSG:4326" style="height: 400px">
      <vl-view :zoom.sync="zoom" :center.sync="center" :rotation.sync="rotation"></vl-view>



      <vl-layer-tile id="osm">
        <vl-source-osm></vl-source-osm>
      </vl-layer-tile>


      <template v-if="from.coordinatesArray">
        <vl-overlay :position=from.coordinatesArray >
          <template slot-scope="scope">
            <div class="overlay-content">
              Hello world!<br>
              Position: {{ scope.coordinates }}
            </div>
          </template>
        </vl-overlay>
      </template>




    </vl-map>

    <div style="padding: 20px">
      Zoom: {{ zoom }}<br>
      Center: {{ center }}<br>
      Rotation: {{ rotation }}<br>
      My geolocation: {{ geolocPosition }}
    </div>
  </div>
</template>

<script>
  export default {
    props : {
      from: {},
      to: {}
    },
    data () {
      return {
        zoom: 8.5,
        center: [125.18578460456244, 7.87327159352715],
        rotation: 0,
        geolocPosition: undefined,
      }
    },
  }
</script>
