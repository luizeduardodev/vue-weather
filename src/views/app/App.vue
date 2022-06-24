<template>
    <div id="app">
        <main>
            <div class="search-box">
                <input
                    type="text"
                    class="search-bar"
                    placeholder="Search..."
                    v-model="querry"
                    @keypress="fetchWeather"
                />
            </div>

            <div class="weather-wrap">
                <div class="location-box">
                    <div class="location">
                        <p>{{ weatherData.city }}</p>
                    </div>

                    <div class="dateAndcurrently" v-if="weatherData.date">
                        <p>
                            {{ weatherData.date }},
                            {{ weatherData.currently }}
                        </p>
                    </div>

                    <div class="weather-box">
                        <div class="temp" v-if="weatherData.temp">
                            <p>{{ weatherData.temp }}°C</p>
                        </div>

                        <div class="weather">
                            <p>{{ weatherData.description }}</p>
                        </div>

                        <div class="wind_speedy" v-if="weatherData.wind_speedy">
                            <p>
                                Ventos com velocidade de
                                {{ weatherData.wind_speedy }}
                            </p>
                        </div>
                    </div>

                    <div class="button-container">
                        <button
                            id="buttonClear"
                            class="buttonStyle"
                            v-on:click="clearData"
                        >
                            Limpar dados
                        </button>
                    </div>
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
            apiKey: "3b98d6f5",
            url: "https://api.hgbrasil.com/weather",
            querry: "",
            weatherData: {}
        };
    },
    methods: {
        fetchWeather(e) {
            if (e.key == "Enter") {
                fetch(
                    `${this.url}?format=json-cors&key=${this.apiKey}&city_name=${this.querry}`
                )
                    .then((res) => {
                        return res.json();
                    })
                    .then((data) => {
                        const results = data.results;
                        this.setWeatherData(results);
                        this.showBtn();
                    });
            }
        },
        setWeatherData(results) {
            this.weatherData = results;
        },
        clearData() {
            const btn = document.getElementById("buttonClear");
            btn.classList.remove("show-btn");

            this.querry = "";
            this.weatherData = "";
        },
        showBtn() {
            const btn = document.getElementById("buttonClear");
            btn.classList.add("show-btn");
        }
    }
};
</script>

<style src="./style.scss" lang="scss" />
