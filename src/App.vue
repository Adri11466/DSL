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
          placeholder="Busca la ciudad..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            <h1>Clima actual en</h1>
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
      <div class="author">
        <span>© Adrián Balsa Puerto 2022</span>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      api_key: "da7db7997ff0a59c8c667fa421c49e97",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },

  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((data) => {
            return data.json();
          })
          .then(this.setResults);
      }
    },

    setResults(results) {
      this.weather = results;
    },

    dateBuilder() {
      let d = new Date();
      let months = [
        "Enero",
        "Febrero",
        "Marzo",
        "Abril",
        "Mayo",
        "Junio",
        "Julio",
        "Agosto",
        "Septiembre",
        "Octubre",
        "Noviembre",
        "Diciembre",
      ];
      let days = [
        "Domingo",
        "Lunes",
        "Martes",
        "Miércoles",
        "Jueves",
        "Viernes",
        "Sábado",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@100;300;500;700;900&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Poppins", sans-serif;
  font-size: 16pt;
}

#app {
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.2s;
}

#app.warm {
  background-image: url("./assets/warm-bg.jpg");
  background-size: cover;
  background-position: top;
  transition: 0.4s;
}

main {
  min-height: 100vh;
  padding: 60px 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );

  .search-box {
    width: 100%;
    margin-bottom: 30px;

    @media screen and (min-width: 768px) {
      width: 50%;
      margin: 0 auto 30px;
    }

    .search-bar {
      display: block;
      width: 100%;
      padding: 15px;
      color: #313131;
      font-size: 1.1rem;
      appearance: none;
      border: none;
      outline: none;
      background: none;
      box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
      background-color: rgba(255, 255, 255, 0.5);
      border-radius: 0px 16px 0px 16px;
      transition: 0.4s;
    }

    .search-bar:focus {
      box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
      background-color: rgba(255, 255, 255, 0.75);
      border-radius: 16px 0px 16px 0px;
    }
  }

  .logo-container {
    max-width: 130px;
    padding: 0px 0px 20px;

    @media screen and (min-width: 768px) {
      max-width: 230px;
      margin: 0 auto;
      padding: 0px 20px 20px;
    }

    img {
      width: 100%;
      height: auto;
    }
  }

  h1 {
    font-size: 1rem;
    font-weight: 300;
  }

  .location-box {
    .location {
      color: #fff;
      font-size: 2rem;
      font-weight: 500;
      text-align: center;
      text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
    }

    .date {
      color: #fff;
      font-size: 1.2rem;
      font-weight: 300;
      font-style: italic;
      text-align: center;
    }
  }

  .author {
    position: absolute;
    bottom: 0;
    width: 100%;
    text-align: center;
    padding: 10px;
    font-weight: 300;
  }

  .author span {
    display: block;
    color: rgb(198, 198, 198);
    font-size: 0.75rem;
    line-height: 1rem;
  }

  .author a {
    color: #fff;
  }

  .weather-box {
    text-align: center;

    .temp {
      display: inline-block;
      padding: 10px 25px;
      color: #fff;
      font-size: 4.5rem;
      font-weight: 700;
      text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
      background-color: rgba(255, 255, 255, 0.25);
      border-radius: 16px;
      margin: 30px 0px;
      box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    }

    .weather {
      color: #fff;
      font-size: 2.5rem;
      font-weight: 500;
      font-style: italic;
      text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    }
  }
}
</style>
