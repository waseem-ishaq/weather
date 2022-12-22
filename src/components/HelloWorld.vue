<template>
  <v-container>
    <div class="width:50% mx-auto pa-4">
      <div class="d-flex">
        <div class="search-filed">
          <v-text-field
          v-model="city"  
          class="rounded-pill"
            outlined
            label="Enter city name here.."
            type="text"
            @change="fetchWeatherApi()"
          />
        </div>
        <div>
          <v-btn @click="fetchWeatherApi()" class="pa-6 rounded-pill info search-btn">
            <h3>search</h3>
          </v-btn>
        </div>
      </div>
    </div>
    <div>
      <div>
        <div class="d-flex">
          <div>
            <h1>{{ this.city }},</h1>
          </div>
          <div>
            <h1>{{ this.country }}</h1>
          </div>
        </div>
        <div>
          <b>{{ this.wCondition }} </b>
        </div>
        <div>{{ this.tem }}C</div>
        <div><b>Humidity :</b>{{ this.humidity }}%</div>
      </div>
      <div>
        <img class="img" v-bind:src="this.imgsrc" />
      </div>
    </div>
  </v-container>
</template>

<script>
export default {
  name: "HelloWorld",

  data() {
    return {
      city: "",
      country: "",
      wCondition: "",
      tem: "",
      imgsrc: "",
      humidity: "",
    };
  },
  methods: {
    fetchWeatherApi() {
      fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=0ca12bf69c2c9bb5091f9d18f3423b19`
      )
        .then((response) => response.json())
        .then((data) => {
          console.log(data);
          this.country = data.sys.country;
          this.wCondition = data.weather[0].description;
          this.tem = data.main.temp;
          this.imgsrc = `https://openweathermap.org/img/wn/${data.weather[0].icon}@2x.png`;
        });
    },
    fetchCityesApi() {
      fetch("https://countriesnow.space/api/v0.1/countries/population/cities")
        .then((response) => response.json())
        .then((data) => {
          console.log(data.data);
          this.city = data.data;
        });
    },
  },
  mounted() {
    fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=lahore&appid=0ca12bf69c2c9bb5091f9d18f3423b19`
    )
      .then((response) => response.json())
      .then((data) => {
        console.log(data);
        this.city=data.name;
        this.country = data.sys.country;
        this.wCondition = data.weather[0].description;
        this.tem = Math.round(data.main.temp - 373);
        this.imgsrc = `https://openweathermap.org/img/wn/${data.weather[0].icon}@2x.png`;
        this.humidity = data.main.humidity;
      });
  },
};
</script>

<style scoped>
.search-filed {
  width: 100%;
}
</style>
