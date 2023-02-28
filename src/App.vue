<template>

<!-- Corregir esto y cambiarle lo de que no sea por temperatura sino por clima -->
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input 
        type="text" 
        class="search-bar" 
        placeholder="Search..."
        v-model="query"
        @keypress="fetchWeather"
        >
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{weather.name}}, {{weather.sys.country}}
          </div>
          <div class="date">
            {{dateBuilder()}}
          </div>
        </div>

        <div class="weather-box">
          <div class="temp">
            {{ Math.round(weather.main.temp)}}ºC
            <div class="min-max">
              <div class="min">
                Min: {{ Math.round(weather.main.temp_min)}}ºC
              </div>
              <div class="max">
                Max: {{ Math.round(weather.main.temp_max)}}ºC
              </div>
            </div>
          </div>
          <div class="weather">
            {{ weather.weather[0].main}}
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data(){
    return {
      api_key: '6d98ae957ac98a4c40735b46f5aa702e',
      url: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather(e){
      if(e.key == "Enter"){
        fetch(`${this.url}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(data => {
            return data.json()
          })
          .then(this.setResults)
      }
    },
    setResults(results){
      this.weather = results;
      console.log(results)
    },
    dateBuilder(){
      let d = new Date();
      let months=["January","February","March","April","May","June","July",
      "August","September","October","November","December"];
      let days=["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday",
      "Saturday"]

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`
    }
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Blinker', sans-serif
}

#app {
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.5s;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpg');
}

main{
  min-height: 100vh;
  padding: 35px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25),
  rgba(0,0,0,0.75));
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-bar{
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  box-shadow: 0px 0px 10px rgba(0,0,0,0.25);
  background-color: rgba(255, 255, 255, 0.45);
  border-radius: 10px;
  transition: 0.5s;
}

.search-bar:focus{
  background-color: rgba(255, 255, 255, 0.6);
  box-shadow: 0px 0px 15px rgba(0,0,0,0.55);
}

.location{
  color: #fff;
  font-size: 40px;
  font-weight: 600;
  text-align: center;
  text-shadow: 0px 3px rgba(0,0,0,0.25);
  margin-bottom: 8px;
} 

.date {
  color: #fff;
  font-size: 22px;
  font-weight: 200;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.temp {
  display: inline-block;
  padding: 10px 25px ;
  color: #fff;
  font-size: 6rem;
  font-weight: 800;
  text-shadow: 0px 4px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.25);
  border-radius: 10px;
  margin: 30px 0px;
  padding: 25px;
  box-shadow: 0px 5px 7px rgba(0,0,0,0.25);
}

.weather{
  color: #fff;
  font-size: 2rem;
  font-weight: 700;
  font-style: italic;
  text-shadow: 0px 3px rgba(0,0,0,0.25);
}

.min-max {
  display: flex;
  justify-content: space-between;
}

.min, .max {
  font-size: 1.2rem;
}
</style>
