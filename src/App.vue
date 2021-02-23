<template>
  <div id="app">
    <!-- <button type="button" class="btn btn-primary">Primary</button> -->
    <div class="row no-gutters">
      <!-- 選擇地區 -->
      <div class="toolbox col-sm-3 p-2 bg-white">
        <div class="form-group d-flex">
          <label for="city" class="col-form-label mr-2 text-right">縣市</label>
          <div class="flex-fill">
            <select name="" id="city" class="form-control" v-model="select.city">
              <!-- 製作下拉選單 -->
              <option :value="city.name" :key="city.name" v-for="city in cityName">
                {{ city.name }}
              </option>
            </select>
          </div>
        </div>
        <div class="form-group d-flex">
          <label for="dist" class="col-form-label mr-2 text-right">地區</label>
          <div class="flex-fill">
            <select name="" id="dist" class="form-control" v-model="select.dist">
              <!-- 製作下拉選單 -->
              <option
                :value="dist.name"
                :key="dist.name"
                v-for="dist in cityName.find((city) => city.name === select.city).districts"
                >{{ dist.name }}</option
              >
            </select>
          </div>
        </div>
      </div>

      <!-- 顯示地圖和 ubike 站點 -->
      <div class="col-sm-9">
        <div id="map"></div>
      </div>
    </div>
  </div>
</template>

<script>
import L from 'leaflet';
import cityName from './assets/cityName.json';

export default {
  name: 'App',
  data: () => ({
    cityName,
    select: {
      city: '臺北市',
      dist: '中正區',
    },
    ubikes: [],
    OSMap: {},
  }),
  computed: {
    youbikes() {
      return this.ubikes.filter((bike) => bike.sarea === this.select.dist);
    },
  },
  watch: {
    youbikes() {
      this.updateMap();
    },
  },
  methods: {
    updateMap() {
      // remove markers
      this.OSMap.eachLayer((layer) => {
        if (layer instanceof L.Marker) {
          this.OSMap.removeLayer(layer);
        }
      });

      // add marker on map
      /* eslint-disable */
      this.youbikes.forEach((bike) => {
        L.marker([bike.lat, bike.lng])
          .bindPopup(
            `<p><strong style="font-size: 20px;">${bike.sna}</strong></p>
            <strong style="font-size: 16px; color: #d45345;">可租借車輛剩餘：${bike.sbi} 台</strong></br>
            可停空位剩餘： ${bike.bemp}</br>
            <small>最後更新時間： ${bike.mday}</small>`
          )
          .addTo(this.OSMap);
      });

      // move to new center
      this.cityName[0].districts.find((dist) => {
        if (dist.name === this.select.dist) {
          // this.OSMap.panTo(new L.LatLng(dist.latitude, dist.longitude));
          this.OSMap.flyTo(new L.LatLng(dist.latitude, dist.longitude), 14);
        }
        return dist.name === this.select.city;
      });
    },
  },
  created() {
    const api = 'https://tcgbusfs.blob.core.windows.net/blobyoubike/YouBikeTP.json';
    this.$http.get(api).then((response) => {
      console.log(response.data);
      this.ubikes = Object.keys(response.data.retVal).map((key) => response.data.retVal[key]);
    });
  },
  mounted() {
    this.OSMap = L.map('map', {
      center: [25.041956, 121.508791],
      zoom: 18,
    });

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution:
        'Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
      maxZoom: 18,
    }).addTo(this.OSMap);
  },
};
</script>

<style lang="scss">
@import 'bootstrap/scss/bootstrap';
#map {
  height: 100vh;
  position: relative;
}
</style>
