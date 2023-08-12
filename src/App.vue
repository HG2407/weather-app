<template>
  <div id="content" :class="( typeof weather.main !== 'undefined' && weather.main.temp > 16 ? 'warm' : '')">
    <div class="container">
      <div class="search-box">
      <input id='search-bar' name="search-bar" type="text" placeholder="Search..." v-model='query' @keypress="getWeather">
      </div>

      <div class="weather-wrap" v-if="(typeof weather.main !== 'undefined')">
      <p class="place-name">{{ weather.name }}, {{ weather.sys.country }}</p>
      <p class="date">{{ findDate() }}</p>

      <div class="temp">{{Math.round(weather.main.temp)}}°c</div>
      <p class="weather">{{ weather.weather[0].main }}</p>

      </div>
      
    </div>
    
  </div>
  
</template>

<script>

export default {
  name: 'App',

  data() {
    return  {
      api_key: '4223e5913f82545b3c54a9bf180277be',
      base_url: 'http://api.openweathermap.org',
      query: '',
      weather: {},
    }
  },

  methods: {
    async getWeather(e) {
      if(e.key == 'Enter') {
        // fetch(`${this.base_url}/data/2.5/weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        // .then(res => {
        //   return res.json();
        // }).then(this.setWeather);

        let response = await fetch(`${this.base_url}/data/2.5/weather?q=${this.query}&units=metric&APPID=${this.api_key}`);
        let json = await response.json();
        this.setWeather(json);
      }
    },
    setWeather(result) {
      this.weather = result;
    },
    findDate() {
      let now = new Date();
      let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
      let months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];

      let day = days[now.getDay()];
      let month = months[now.getMonth()];
      let year = now.getFullYear();
      let date = now.getDate();

      return `${day} ${date} ${month} ${year}`;
    }
  }
  
  
}
</script>

<style>

#content {
  width: 100%;
  height: 100%;
  background-image: url('./assets/winter-bg.jpg');
  background-size: cover; 
  text-align: center;
  color: #fff;
  font-family:'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  transition: 0.4s;
}

.container {
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.2));
  height: 100%;
}

#search-bar {
  margin-top: 2rem;
  opacity: 0.7;
  background-color: rgba(255, 255, 255, 0.5);
  border-width: 0;
  font-size: 1.2rem;
  padding-left: 0.6rem;
  border-radius: 0 1rem 0 1rem;
  color: white;
}

#search-bar:focus-visible {
  box-shadow: 0.2rem 0.2rem 0.4rem rgb(30, 25, 25);
  outline-width:0;
  border-radius: 1rem 0 1rem 0;
  transition: 0.4s;
  opacity: 0.8;
}

.temp {
  margin-top: 1rem;
  display: inline-flex;
  width: 8rem;
  height: 5rem;
  background-color:rgba(211, 211, 211, 0.5);
  border-radius: 0.3rem;
  justify-content: center;
  align-items: center;
  color: #fff;
  text-shadow: 0.1rem 0.1rem 0.1rem rgb(139, 137, 137);
  font-size: 3rem;
  font-weight: bolder;
  box-shadow: 0.15rem 0.15rem 0.2rem  rgba(39, 38, 38, 0.5);
}

.place-name {
  margin-top: 2rem;
  margin-bottom: 1rem;
  font-size: 2rem;
  font-weight: bolder;
  text-shadow: 0rem 0.1rem 0.1rem rgba(0, 0, 0, 1);
  
}

.date {
  font-size: 1.1rem;
  font-style: italic;

  color: #fff;
}

.weather {
  margin-top: 1.5rem;
  font-size: 2rem;
  text-shadow: 0rem 0.14rem 0.1rem rgb(39, 38, 38);
  color:#fff;
}

#content.warm {
  background-image: url('./assets/warm-bg.jpg');
  transition: 0.4s;
}
 
</style>
