<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <h1>Descubra a temperatura atual da sua cidade!</h1>
        <v-text-field
          v-model="newCity"
          label="Insira o nome da sua cidade"
          @keyup.enter="addCity"
        ></v-text-field>
        <v-row>
          <v-col
            v-for="(data, index) in responses"
            :key="index"
            cols="12"
            sm="6"
            md="4"
          >
            <v-card>
              <v-img
                :src="getWeatherImage(data.weather)"
                height="200px"
                class="white--text align-end"
              >
                <v-card-title>{{
                  data.city.charAt(0).toUpperCase() + data.city.slice(1)
                }}</v-card-title>
              </v-img>
              <v-card-text>Temperatura: {{ data.temperature }}°C</v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      capitals: [
        "Rio Branco",
        "Maceió",
        "Macapá",
        "Manaus",
        "Salvador",
        "Fortaleza",
        "Brasília",
        "Vitória",
        "Goiânia",
        "São Luís",
        "Cuiabá",
        "Campo Grande",
        "Belo Horizonte",
        "Belém",
        "João Pessoa",
        "Curitiba",
        "Recife",
        "Teresina",
        "Rio de Janeiro",
        "Natal",
        "Porto Alegre",
        "Porto Velho",
        "Boa Vista",
        "Florianópolis",
        "São Paulo",
        "Aracaju",
        "Palmas",
      ],
      initialCapitals: [
        "Caruaru",
        "Porto Alegre",
        "São Paulo",
        "Rio de Janeiro",
        "Brasília",
      ],
      responses: [],
      newCity: "",
    };
  },
  mounted() {
    this.fetchInitialData();
  },
  methods: {
    async fetchInitialData() {
      const apiKey = "fd9ef5cbc248c2089645da454394cb7f";
      const promises = this.initialCapitals.map((city) => {
        const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric`;
        return axios.get(url).then((response) => ({
          city,
          temperature: response.data.main.temp,
          weather: response.data.weather[0],
        }));
      });

      try {
        const results = await Promise.all(promises);
        this.responses = results;
      } catch (error) {
        console.error("Erro ao buscar dados das capitais:", error);
      }
    },
    async addCity() {
      if (
        this.newCity &&
        !this.responses.find(
          (response) =>
            response.city.toLowerCase() === this.newCity.toLowerCase()
        )
      ) {
        const apiKey = "fd9ef5cbc248c2089645da454394cb7f";
        const url = `https://api.openweathermap.org/data/2.5/weather?q=${this.newCity}&appid=${apiKey}&units=metric`;

        try {
          const response = await axios.get(url);
          this.responses.push({
            city: this.newCity,
            temperature: response.data.main.temp,
            weather: response.data.weather[0],
          });
          this.newCity = "";
        } catch (error) {
          console.error("Erro ao adicionar cidade:", error);
        }
      }
    },
    getWeatherImage(weather) {
      const weatherImages = {
        Thunderstorm:
          "https://cdn.icon-icons.com/icons2/1903/PNG/512/iconfinder-weather-weather-forecast-lightning-cloud-storm-3859137_121212.png",
        Drizzle:
          "https://cdn.icon-icons.com/icons2/1903/PNG/512/iconfinder-weather-weather-forecast-heavy-rain-cloud-climate-3859135_121235.png",
        Rain: "https://cdn.icon-icons.com/icons2/1903/PNG/512/iconfinder-weather-weather-forecast-heavy-rain-cloud-climate-3859135_121235.png",
        Snow: "https://cdn.icon-icons.com/icons2/1903/PNG/512/iconfinder-weather-weather-forecast-flake-flakes-snowflake-3859134_121231.png",
        Clear:
          "https://cdn.icon-icons.com/icons2/1903/PNG/512/iconfinder-weather-weather-forecast-hot-sun-day-3859136_121222.png",
        Clouds:
          "https://cdn.icon-icons.com/icons2/1903/PNG/512/iconfinder-weather-weather-forecast-cloudy-season-cloud-3859132_121213.png",
      };
      return (
        weatherImages[weather.main] ||
        "https://cdn.icon-icons.com/icons2/1903/PNG/512/iconfinder-weather-weather-forecast-cloudy-season-cloud-3859132_121213.png"
      );
    },
  },
};
</script>

<style scoped>
h1 {
  margin-bottom: 20px;
}
</style>
