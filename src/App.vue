<script>
import axios from "axios";

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null,
    };
  },
  computed: {
    cityName() {
      return "«" + this.city + "»";
    },
    showTemp() {
      return "Temperature: " + this.info.main.temp;
    },
    showFellsLike() {
      return "Feels like: " + this.info.main.feels_like;
    },
    showMinTemp() {
      return "Minimum temperature: " + this.info.main.temp_min;
    },
    showMaxTemp() {
      return "Maximum temperature: " + this.info.main.temp_max;
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Please enter more than one character name";
        return false;
      }

      this.error = "";

      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=94dbe5bb3cb290f2f9bf83dff2f056dd`
        )
        .then((res) => (this.info = res.data));
    },
  },
};
</script>

<template>
  <div class="wrapper">
    <h1>Weather app</h1>
    <p>Check the weather in {{ city === "" ? "your city" : cityName }}</p>
    <div>
      <input type="text" placeholder="Enter city" v-model="city" />
      <button v-if="city !== ''" @click="getWeather()">Get the weather</button>
      <button disabled v-else>Enter the name of city</button>
    </div>
    <p class="error">{{ error }}</p>

    <div v-if="info !== null">
      <p>{{ showTemp }}</p>
      <p>{{ showFellsLike }}</p>
      <p>{{ showMinTemp }}</p>
      <p>{{ showMaxTemp }}</p>
    </div>
  </div>
</template>

<style scoped>
.error {
  color: #d03939;
}

.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background-color: #1f0f24;

  color: #fff;

  display: flex;
  flex-direction: column;
  align-items: center;
}

.wrapper h1 {
  margin-top: 50px;
}

.wrapper p {
  margin-top: 20px;
}

.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: #6e2d7d;
}

.wrapper button:disabled {
  background: #746027;
  cursor: not-allowed;
}

.wrapper button {
  background: #e3dc4b;
  color: #fff;
  border-radius: 10px;
  border: 2px solid #b99935;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}
</style>
