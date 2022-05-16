<template>
  <div class="main-page">
    <header class="search-box w3-container">
      <input class="w3-twothird" type="text" placeholder="Pesquise uma cidade..." v-model="query" @keypress="fetchWeather" />
    </header>
    <main>

      <section>
        <div class="w3-container box-error" v-if="typeof weather.list == 'undefined' && isError == true">
          <div class="w3-half undefined-city">
            <div class="w3-center title">Cidade não encontrada!</div>
            <div class="w3-center subtitle"><i>Por favor, informe uma cidade válida.</i></div>
        </div>
        </div>


        <div class="w3-container weather-box" v-if= "typeof weather.list != 'undefined'">
          <div id="warm-cold" class="w3-quarter defined-city" :class="typeof weather.list != 'undefined' && weather.list[0].main.temp > 15 ? 'warm' : '' "> 
            <div class="w3-twothird w3-center card-defined-city">
              <div class="card-title"> {{ weather.city.name }}, {{ weather.city.country }} </div>
              <div class="card-date">{{ dateBuilder() }}</div>
                  <div class="main-temp">
                      <div><img v-bind:src="icones[0]" /></div>
                      <div class="weather-temp"> {{ Math.round(weather.list[0].main.temp) }}<sup>ºC</sup> </div>
                  </div>
                <!--<div class="weather-description">{{ weather.list[0].weather[0].description }}</div>-->
                <div class="subtitle"> Sensação Térmica: <strong>{{ Math.round(weather.list[0].main.feels_like) }}ºC</strong></div>
              <hr>
              <div class="weather-info">
                <div class="temp_min">
                  <img src="../assets/temp_min.svg" />
                  <p>{{ Math.round(weather.list[0].main.temp_min) }}<sup>ºC</sup></p>
                </div>
                <div class="temp_max">
                  <img src="../assets/temp_max.svg" />
                  <p>{{ Math.round(weather.list[0].main.temp_max) }}<sup>ºC</sup></p>
                </div>
                <div class="humidity">
                  <img src="../assets/humidity.svg" />
                  <p>{{ Math.round(weather.list[0].main.humidity) }}%</p>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div>
        <div class="w3-container w3-center w3-xlarge other-days"  v-if= "typeof weather.list != 'undefined'">
          <section class="w3-bar card-days">
            <div class="w3-bar-item w3-mobile card-day ">
              <div>{{ timestamp[1] }}</div>
              <div><img v-bind:src="icones[1]" /></div>
                <div class="weather-info">
                  <div class="day-temp_min">
                    <img src="../assets/temp_min.svg" />
                    <p>{{ Math.round(min_temp[1]) }}<sup>ºC</sup></p>
                  </div>
                  <div class="day-temp_max">
                    <img src="../assets/temp_max.svg" />
                    <p>{{ Math.round(max_temp[1]) }}<sup>ºC</sup></p>
                  </div>  
              </div>
            </div>

            <div class="w3-bar-item w3-mobile card-day">
              <div>{{ timestamp[2] }}</div>
              <div>
                <img v-bind:src="icones[2]" />
              </div>
                <div class="weather-info">
                  <div class="day-temp_min">
                    <img src="../assets/temp_min.svg" />
                    <p>{{ Math.round(min_temp[2]) }}<sup>ºC</sup></p>
                  </div>
                  <div class="day-temp_max">
                    <img src="../assets/temp_max.svg" />
                    <p>{{ Math.round(max_temp[2]) }}<sup>ºC</sup></p>
                  </div>
                </div>
              
            </div>

            <div class="w3-bar-item w3-mobile card-day">
              <div>{{ timestamp[3] }}</div>
              <div><img v-bind:src="icones[3]" /></div>
                  <div class="weather-info">
                    <div class="day-temp_min">
                      <img src="../assets/temp_min.svg" />
                      <p>{{ Math.round(min_temp[3]) }}<sup>ºC</sup></p>
                    </div>
                    <div class="day-temp_max">
                      <img src="../assets/temp_max.svg" />
                      <p>{{ Math.round(max_temp[3]) }}<sup>ºC</sup></p>
                    </div>
                </div>
            </div>

            <div class="w3-bar-item w3-mobile card-day">
              <div>{{ timestamp[4] }}</div>
              <div>
                <img v-bind:src="icones[4]" />
              </div>
                <div class="weather-info">
                  <div class="day-temp_min">
                    <img src="../assets/temp_min.svg" />
                    <p>{{ Math.round(min_temp[4]) }}<sup>ºC</sup></p>
                  </div>
                  <div class="day-temp_max">
                    <img src="../assets/temp_max.svg" />
                    <p>{{ Math.round(max_temp[4]) }}<sup>ºC</sup></p>
                  </div>
                </div>
            </div>

             <div class="w3-bar-item w3-mobile card-day">
              <div>{{ timestamp[5] }}</div>
              <div>
                <img v-bind:src="icones[5]" />
              </div>
            
                <div class="weather-info">
                  <div class="day-temp_min">
                    <img src="../assets/temp_min.svg" />
                    <p>{{ Math.round(min_temp[5]) }}<sup>ºC</sup></p>
                  </div>
                  <div class="day-temp_max">
                    <img src="../assets/temp_max.svg" />
                    <p>{{ Math.round(max_temp[5]) }}<sup>ºC</sup></p>
                  </div>
                </div>
            </div>
          </section>
          </div>
        </div> 
      </section>
    </main>
  </div>
</template>


<script>
export default {
  name: "MainPage",
  components: {
    
  },

  data() {
    return {
      api_key: "ec40f5e4d97b396e14629aaf3426614f", //Inserir a chave da API AQUI 
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      isError: false,
      icones: [],
      dias: [0, 1, 9, 17, 25, 33],
      max_temp: [],
      min_temp: [],
      timestamp: []
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        (this.isError = true),
          fetch(
            `${this.url_base}forecast?q=${this.query}&lang=pt&units=metric&APPID=${this.api_key}`
          )
            .then((res) => {
              return res.json();
            })
            .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;

      for (let j = 0; j < 6; j++) {
        this.icones[j] = `http://openweathermap.org/img/wn/${
          this.weather.list[this.dias[j]].weather[0].icon
        }@2x.png`;
        //this.year = this.weather.list[this.dias[j]].dt_txt.slice(0, 4);
      this.month = this.weather.list[this.dias[j]].dt_txt.slice(5, 7);
      this.dia = this.weather.list[this.dias[j]].dt_txt.slice(8, 10);
      this.timestamp[j] = this.dia + "/" + this.month;

      this.temporary_list = 100000;
        this.temporary_list_max = -10000;
        for (let i = 0; i < 7; i++) {
          if (
            this.weather.list[this.dias[j+1] + i].main.temp_min <
            this.temporary_list
          ) {
            this.temporary_list =
              this.weather.list[this.dias[j+1] + i].main.temp_min;
          }
          if (
            this.weather.list[this.dias[j+1] + i].main.temp_max >
            this.temporary_list
          ) {
            this.temporary_list_max =
              this.weather.list[this.dias[j+1] + i].main.temp_max;
          }
        }
        this.min_temp[j+1] = this.temporary_list;
        this.max_temp[j+1] = this.temporary_list_max;
      }

    },
    dateBuilder() {
      let d = new Date();
      let months = ["Janeiro", "Fevereiro", "Março", "Abril", "Maio", "Junho", "Julho", "Agosto", "Setembro", "Outubro", "Novembro", "Dezembro"];
      let days = ["Domingo", "Segunda", "Terça", "Quarta", "Quinta", "Sexta", "Sábado"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day}, ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background-color: var(--bg-body);
}

.search-box {
  margin: 2rem 0 4rem;
  display: flex;
  justify-content: center;
}

.search-box input {
  background-color: var(--bg-card);
  border: 1px solid var(--bg-card);
  color: var(--white);
  padding: 1.5vh;
  border-radius: 15px;
  font-size: 14px;
}

.box-error,
.weather-box {
  flex-direction: row;
}

.box-error,
.weather-box,
.undefined-city,
.defined-city,
.main-temp,
.weather-temp{
  display: flex;
  justify-content: center;
  align-items: center;
}

.undefined-city {
  background: var(--bg-orange-gradient);
  height: 40vh;
  border-radius: 30px;
  flex-direction: column; 
}

#warm-cold{
  background-image: url(../assets/cold-bg.jpg);
  margin: auto;
  padding: 1rem;
  border-radius: 20px;
}

#warm-cold.warm{
  background-image: url(../assets/warm-bg.jpg);

}

.title, 
.subtitle {
  font-family: var(--font-type-one);
}

.title {
  font-size: 4vh;
  font-weight: bold;
}

.subtitle {
  font-size: 2vh;
}

.card-title, 
.card-date {
  font-family: var(--font-type-one);
}
.card-title {
  font-size: 3vh;
  font-weight: bold;
}
.card-date {
  font-size: 1.8vh;
  font-style: italic;
}

.main-temp {
  padding: 1rem;
  font-size: 6vh;
  font-family: var(--font-type-one);
}

.weather-info {
  display: flex;
  justify-content: space-around;
}

.weather-info p {
  display: inline-block;
  margin-left: 1px;
  font-size: 14pt;
}

.other-days {
  display: flex;
  justify-content: space-around;
  margin-top: 2rem;
}

.card-days {
  margin-right: 2rem;
  padding: 1.5rem;
  border-radius: 16px;
}

.card-day{
  background: var(--bg-blue-gradient);
  font-family: var(--font-type-one);
  margin: 1rem;
  border-radius: 16px;
}

.day-temp_max {
  margin-left: 2rem
}

</style>
