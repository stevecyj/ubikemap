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
import cityName from './assets/cityName.json';

export default {
  name: 'App',
  data: () => ({
    cityName,
    select: {
      city: '臺北市',
      dist: '中正區',
    },
  }),
  created() {
    const api = 'https://tcgbusfs.blob.core.windows.net/blobyoubike/YouBikeTP.json';
    this.$http.get(api).then((response) => {
      console.log(response.data);
    });
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
