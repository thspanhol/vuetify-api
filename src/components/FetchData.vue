<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <h1>Dados das Capitais</h1>
        <v-text-field
          v-model="newCity"
          label="Digite o nome de uma cidade"
          @keyup.enter="addCity"
        ></v-text-field>
        <v-list>
          <v-list-item v-for="(data, index) in responses" :key="index">
            <v-list-item-content>
              <v-list-item-title>Capital: {{ data.city }}</v-list-item-title>
              <v-list-item-subtitle
                >Temperatura: {{ data.temperature }}°C</v-list-item-subtitle
              >
            </v-list-item-content>
          </v-list-item>
        </v-list>
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
      initialCapitals: ["Rio Branco", "Maceió", "Macapá", "Manaus", "Salvador"],
      responses: [],
      newCity: "",
    };
  },
  mounted() {
    this.fetchInitialData();
  },
  methods: {
    async fetchInitialData() {
      const apiKey = "fd9ef5cbc248c2089645da454394cb7f"; // Substitua pela sua chave de API
      const promises = this.initialCapitals.map((city) => {
        const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric`;
        return axios.get(url).then((response) => ({
          city,
          temperature: response.data.main.temp,
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
          });
          this.newCity = "";
        } catch (error) {
          console.error("Erro ao adicionar cidade:", error);
        }
      }
    },
  },
};
</script>

<style scoped>
h1 {
  margin-bottom: 20px;
}
</style>
