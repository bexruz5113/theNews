<template>
  <v-container fluid>
    <v-row class="d-lg-flex d-block">
      <v-col cols="12" sm="6" class="pb-0 d-flex align-center justify-start">
        <div class="d-sm-flex align-center d-block">
          <div class="d-flex align-center">
            <div
              v-for="(getMain, index) in getWeatherInfo.weather"
              :key="index"
            >
              <v-img
                v-if="getMain.decoration == 'light rain'"
                width="20"
                src="../assets/light-rain.png"
              >
              </v-img>
              <v-img
                v-else-if="getMain.description == 'overcast clouds'"
                width="20"
                src="../assets/overcast clouds.png"
              >
              </v-img>
              <v-img
                v-else-if="getMain.description == 'few clouds'"
                width="20"
                src="../assets/few cloud.png"
              >
              </v-img>
              <v-img
                v-else-if="getMain.description == 'broken clouds'"
                width="20"
                src="../assets/broken clouds.png"
              >
              </v-img>

              <v-img v-else width="20" src="../assets/clear sky.png"></v-img>
            </div>
            <div class="b-block mx-3">
              <div>
                <b v-if="getWeatherInfo.main && getWeatherInfo.main.temp"
                  >+{{ calculate }} °C
                </b>
              </div>
            </div>
            <div class="mx-2">
              <v-img width="20" src="../assets/wind.png"></v-img>
            </div>
            <div>
              <b v-if="getWeatherInfo.wind && getWeatherInfo.wind.speed">
                {{ getWeatherInfo.wind.speed }} m/s
              </b>
            </div>
          </div>

          <div>
            <form>
              <v-select
                class="bottomNone"
                @change="onChange()"
                v-model="country"
                :items="items"
                item-text="state"
                item-value="value"
              ></v-select>
            </form>
          </div>
        </div>
      </v-col>
      <v-col
        cols="12"
        sm="6"
        class="pb-0 d-sm-flex d-block align-center justify-lg-end justify-start"
      >
        <div class="d-flex align-center justify-space-between">
          <h3>USD</h3>
          <span class="px-sm-2 px-1">{{ usd.conversion_rate }}</span>
          <v-icon color="green">mdi-trending-up</v-icon>
        </div>

        <div class="d-flex px-sm-4 px-0 align-center justify-space-between">
          <h3>RUB</h3>
          <span class="px-sm-2 px-1"> {{ rub.conversion_rate }}</span>
          <v-icon color="green">mdi-trending-up</v-icon>
        </div>
        <div class="d-flex align-center justify-space-between">
          <h3>EUR</h3>
          <span class="px-sm-2 px-1">{{ eur.conversion_rate }}</span>
          <v-icon color="red">mdi-trending-down</v-icon>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { mapActions, mapGetters } from "vuex";

export default {
  data: () => ({
    country: "tashkent",
    items: [
      { state: "Toshkent", value: "tashkent" },
      { state: "Andijon", value: "andijan" },
      { state: "Jizzax", value: "jizzakh" },
      { state: "Samarqand", value: "samarkand" },
      { state: "Buxoro", value: "bukhara" },
      { state: "Nukus", value: "nukus" },
      { state: "Xiva", value: "khiva" },
      { state: "Navoiy", value: "navoi" },
      { state: "Termiz", value: "termez" },
      { state: "Namangan", value: "namangan" },
      { state: "Guliston", value: "guliston" },
      { state: "Qarshi", value: "qarshi" },
      { state: "Farg'ona", value: "fergana" },
    ],
  }),
  created() {
    this.getWeather(this.country);
    this.getUsd();
    this.getRub();
    this.getEur();
  },
  computed: {
    ...mapGetters("getWeatherInfo", ["getWeatherInfo"]),
    ...mapGetters("forex", ["usd", "rub", "eur"]),
    calculate() {
      return Math.floor(this.getWeatherInfo.main.temp - 273.15);
    },
  },
  methods: {
    ...mapActions("getWeatherInfo", ["getWeather"]),
    ...mapActions("forex", ["getUsd", "getRub", "getEur"]),

    onChange() {
      this.getWeather(this.country);
      console.log("The new value is: ", this.country);
    },
  },
};
</script>

<style lang="scss">
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  text-decoration: none;
}
.bottomNone {
  width: 200px;
  margin-left: 10px;
}
</style>
