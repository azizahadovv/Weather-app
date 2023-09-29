<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''
    "
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="quary"
          @keypress="ShowWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }} ,{{ weather.sys.country }}
          </div>
          <div class="data">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      api_key: "5e2941a4f005685d46bb80f98a748539",
      base_url: `https://api.openweathermap.org/data/2.5/`,
      quary: "",
      weather: {},
    };
  },
  methods: {
    ShowWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.base_url}/weather?q=${this.quary}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResult);
      }
    },
    setResult(result) {
      this.weather = result;
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
        "Sunday",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return ` ${day}, ${date},  ${month}, ${year} `;
    },
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "monserrat", sans-serif;
}
#app {
  background-image: url(./Pictures/cloudyCity.jpg);
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
  transition: 0.5s linear;
}


#app .warm{
  background-image: url(./Pictures/sunny.jpg);
}



main {
  width: 100%;
  height: 100vh;
  padding: 25px;
  display: flex;
  align-items: stretch;
  justify-content: space-evenly;
  flex-wrap: wrap;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.6)
  );
}

.search-box {
  width: 40%;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 5px;
  color: #3a3333;
  font-size: 20px;
  bottom: none;
  background: none;
  outline: none;
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0 16px 0 16px;
  transition: 0.4s;
  border: none;
  margin-top: 30vh;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
}

.location-box .locatin,
.location {
  margin-top: 10vh;
  color: white;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.weather {
  color: white;
  font-size: 42px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .data {
  color: white;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 20px;
  color: white;
  font-size: 100px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

@media screen and (max-width: 480px) {
  main {
    width: 100%;
    height: 100vh;
    padding: 25px;
    display: flex;
    flex-wrap: wrap;
    background-image: linear-gradient(
      to bottom,
      rgba(0, 0, 0, 0.25),
      rgba(0, 0, 0, 0.6)
    );
  }

  .search-box {
    width: 100%;
  }

  .search-box .search-bar {
    display: block;
    width: 100%;
    padding: 5px;
    color: #3a3333;
    font-size: 20px;
    bottom: none;
    background: none;
    outline: none;
    box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 0 16px 0 16px;
    transition: 0.4s;
    border: none;
    margin-top: 10vh;
  }

  .location {
    margin-top: 3vh;
    color: white;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }
}
</style>
