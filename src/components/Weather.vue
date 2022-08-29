<template>
  <div
    id="bg"
    :class="
      typeof waether.main != undefined && water.main.temp_min > 16 ? warm : ''
    "
  >
    <main class="" style="">
      <div class="searchbox mt-6 md:mt-20">
        <input
          type="text"
          placeholder="search..."
          class="searchbar rounded-tr-xl rounded-bl-xl focus:rounded-tl-xl focus:rounded-br-xl focus:rounded-tr-none focus:rounded-bl-none w-64 md:w-96 h-10 md:h-14 block text-gray-900 placeholder-slate-500 md:placeholder:pl-10 outline-none border-none appearance-none bg-none"
          v-model="query"
          @keypress="getWeather"
        />
      </div>
      <div
        class="weather-wrap flex flex-col justify-center items-center space-y-10 mt-10 md:mt-10"
        v-if="typeof weather.main != 'undefined'"
      >
        <div class="location-box">
          <h1
            class="location text-xl md:text-3xl font-bold text-white text-center"
          >
            {{ weather.name }},
            {{ weather.sys.country }}
          </h1>
          <h1
            class="date text-center text-white font-semibold italic text-xl pt-4"
          >
            {{ setDate() }}
          </h1>
        </div>

        <div class="weather-degree rounded-md">
          <h1
            class="text-center font-bold text-3xl md:text-7xl inline-block pt-5 pb-6 px-3 md:px-5 text-white"
          >
            {{ Math.round(weather.main.temp_min) }}&deg;C /
            {{ Math.round(weather.main.temp_max) }}&deg;C
          </h1>
        </div>

        <div class="weather-type">
          <h1
            class="weather-name text-white text-xl md:text-4xl font-bold text-center italic capitalize"
          >
            {{ weather.weather[0].main }}
          </h1>
        </div>
      </div>
    </main>
  </div>
</template>
<script>
export default {
  data() {
    return {
      api_key: "bcd4f6059d5c126286eed5443c6af9bd",
      base_url: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      months: [
        "january",
        "february",
        "march",
        "april",
        "may",
        "june",
        "july",
        "august",
        "september",
        "october",
        "november",
        "december",
      ],
      days: [
        "sunday",
        "monday",
        "tuesday",
        "wednesday",
        "thursday",
        "friday",
        "saturday",
      ],
    };
  },
  methods: {
    getWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.base_url}weather?q=${this.query}&units=metric&appid=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          // console.log(res);
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
      // console.log(this.weather);
    },
    setDate() {
      let d = new Date();
      let month = this.months[d.getMonth()];
      let day = this.days[d.getDay()];
      let date = d.getDate();
      let year = d.getFullYear();
      return `${day} ${date} ${month}, ${year}`;
    },
  },
};
</script>
<style scoped>
#bg {
  background-image: url("../assets/images/weather.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}
.searchbox {
  width: 100%;
  margin-bottom: 30px;
}
.searchbox .search-bar {
  background-color: rgba(255, 255, 255, 0.5);
  transition: 0.4s;
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
}
.searchbox .search-bar :focus {
  background-color: rgba(255, 255, 255, 0.25);
  box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
}
.location-box .location {
  text-shadow: rgba(0, 0, 0, 0.25);
}
.weather-wrap .weather-degree {
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  box-shadow: 3px 6px rgba(0, 0, 0, 0.5);
}
.weather-type .weather-name {
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
/* .warm{
  background-image: url('@/assets/');
} */
@media screen {
  /* main{
    
  } */
}
</style>
