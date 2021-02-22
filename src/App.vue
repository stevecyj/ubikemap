<template>
  <div id="app">
    <button type="button" class="btn btn-primary">Primary</button>
    <div class="row no-gutters">
      <!-- 選擇地區 -->
      <div class="toolbox col-sm-3 p-2 bg-white">
        <div class="form-group d-flex">
          <label for="city" class="col-form-label mr-2 text-right">縣市</label>
          <div class="flex-fill">
            <select name="" id="city" class="form-control">
              <!-- 製作下拉選單 -->
              <option :value="" :key="" v-for="">
                {{}}
              </option>
            </select>
          </div>
        </div>
        <div class="form-group d-flex">
          <label for="dist" class="col-form-label mr-2 text-right">地區</label>
          <div class="flex-fill">
            <select name="" id="dist" class="form-control">
              <!-- 製作下拉選單 -->
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
    select,
  }),
};
</script>

<style lang="scss">
@import 'bootstrap/scss/bootstrap';
#map {
  height: 100vh;
  position: relative;
}
</style>
