<template>
  <div class="home">
    <div>
      <button @click="focusCurrentLocation" type="button">
        Focus to Current Location
      </button>
    </div>
    <div id="map" ref="map">
      <div id="markers" v-for="marker in markers" :key="marker.id">
        <MapMarker :lat="marker.lat" :lng="marker.lng" />
      </div>
    </div>

    <div
      id="infoWindows"
      v-for="(infoWindow, index) in infoWindows"
      :key="index"
    >
      <MapInfoWindow :lat="infoWindow.lat" :lng="infoWindow.lng">
        #{{ index + 1 }}. Info Window
      </MapInfoWindow>
    </div>

    <MapInfoWindow
      v-if="showCurrentLocation"
      :lat="currentLocation.lat"
      :lng="currentLocation.lng"
    >
      <img src="../assets/img/its-you.webp" alt="It's you!" width="100" />
    </MapInfoWindow>
  </div>
</template>

<script>
import MapInfoWindow from '../components/MapInfoWindow';
import MapMarker from '../components/MapMarker';
import { mapSettings } from '../constants/mapSettings';

export default {
  components: {
    MapInfoWindow,
    MapMarker,
  },
  data: () => ({
    map: null,
    mapCenterLocation: { lat: 38.8758834, lng: 32.9297111 },
    currentLocation: { lat: 38.8758834, lng: 32.9297111 },
    showCurrentLocation: false,
    markers: [
      {
        id: 0,
        lat: 41.0156723,
        lng: 28.9563204,
      },
      {
        id: 1,
        lat: 38.4130113,
        lng: 27.2388686,
      },
      {
        id: 2,
        lat: 39.7586378,
        lng: 30.468149,
      },
    ],
    infoWindows: [],
  }),
  methods: {
    getMap(callback) {
      let vm = this;
      function checkForMap() {
        if (vm.map) callback(vm.map);
        else {
          console.log('Checking for map...');
          setTimeout(checkForMap, 2000);
        }
      }
      checkForMap();
    },
    focusCurrentLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          (position) => {
            const pos = {
              lat: position.coords.latitude,
              lng: position.coords.longitude,
            };

            this.mapCenterLocation = pos;
            this.map.setCenter(this.mapCenterLocation);

            this.currentLocation.lat = pos.lat;
            this.currentLocation.lng = pos.lng;

            this.showCurrentLocation = true;
          },
          (errorObj) => {
            console.log(errorObj);
            this.map.setCenter(this.mapCenterLocation);
          },
          { enableHighAccuracy: true, maximumAge: 0, timeout: Infinity }
        );
      } else {
        (errorObj) => {
          alert('This browser does not support GeoLocation API');
          console.log('GeoLocation API Error:', errorObj);
          this.map.setCenter(this.mapCenterLocation);
        };
      }
    },
  },
  mounted() {
    this.map = new window.google.maps.Map(this.$refs.map, {
      ...mapSettings,
      center: this.mapCenterLocation,
      zoom: 7,
    });

    this.map.addListener('dblclick', (event) => {
      console.log('map dbl click', event);
    });

    this.focusCurrentLocation();
  },
};
</script>

<style>
#map {
  height: 95vh;
  background: gray;
}
</style>
