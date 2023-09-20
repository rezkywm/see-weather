<template>
  <ion-app>
    <ion-content class="ion-padding" v-if="weatherData">
      <ion-searchbar
        class="ion-padding"
        color="light"
        placeholder="Enter Location"
        @keyup.enter="updateCityName"
      ></ion-searchbar>
      <div class="we-are-center">
        <div class="start">
          <ion-chip>
            <svg
              xmlns="http://www.w3.org/2000/svg"
              height="24"
              viewBox="0 -960 960 960"
              width="24"
            >
              <path
                d="M480-186q122-112 181-203.5T720-552q0-109-69.5-178.5T480-800q-101 0-170.5 69.5T240-552q0 71 59 162.5T480-186Zm0 79q-14 0-28-5t-25-15q-65-60-115-117t-83.5-110.5q-33.5-53.5-51-103T160-552q0-150 96.5-239T480-880q127 0 223.5 89T800-552q0 45-17.5 94.5t-51 103Q698-301 648-244T533-127q-11 10-25 15t-28 5Zm0-453Zm0 80q33 0 56.5-23.5T560-560q0-33-23.5-56.5T480-640q-33 0-56.5 23.5T400-560q0 33 23.5 56.5T480-480Z"
              />
            </svg>
            <p v-if="cityNotFound">Not found or misspelled.</p>
            <p v-else>{{ weatherData.name }}</p>
          </ion-chip>
          <p>{{ formatDate(currentDate) }}</p>
        </div>
        <main>
          <div class="current-weather">
            <svg
              v-if="
                weatherData.weather[0].icon === '01d' ||
                weatherData.weather[0].icon === '01n'
              "
              xmlns="http://www.w3.org/2000/svg"
              height="64"
              viewBox="0 -960 960 960"
              width="64"
            >
              <path
                d="M479.825-761q-18.85 0-31.838-13.112Q435-787.225 435-806v-90q0-19.2 13.158-32.6 13.158-13.4 32-13.4t32.342 13.4Q526-915.2 526-896v90q0 18.775-13.45 31.888Q499.099-761 479.825-761ZM679-679q-14-12-14-30.9t14-32.1l63-64q12.067-13 31.333-13Q792.6-819 806-806q13 13 13 32t-13 31l-64 64q-13 14-32 14t-31-14Zm127 244q-18.775 0-31.888-13.158-13.112-13.158-13.112-32t13.112-32.342Q787.225-526 806-526h90q19.2 0 32.6 13.45 13.4 13.451 13.4 32.725 0 18.85-13.4 31.838Q915.2-435 896-435h-90ZM479.825-19q-18.85 0-31.838-13.112Q435-45.225 435-64v-90q0-19.2 13.158-32.6 13.158-13.4 32-13.4t32.342 13.4Q526-173.2 526-154v90q0 18.775-13.45 31.888Q499.099-19 479.825-19ZM219-679l-64-63q-14-12.043-14-31.022Q141-792 155.391-806 168-819 186.5-819t31.5 13l64 64q14 14 14 33t-14 31q-13.364 14-32.182 14T219-679Zm523 524-64-64q-14-11.8-13.5-31.15Q665-269.5 679-283q12.5-13 31.25-13T742-283l65 63q13 13 13.129 32.603.13 19.604-13.026 32.5Q793.772-141 774.386-141T742-155ZM64-435q-18.775 0-31.888-13.158Q19-461.316 19-480.158T32.112-512.5Q45.225-526 64-526h90q19.2 0 32.6 13.45 13.4 13.451 13.4 32.725 0 18.85-13.4 31.838Q173.2-435 154-435H64Zm91 279.609Q141-168 141-186.5t14-32.5l64-64q13.5-13 32.25-13T283-282.75q13 13.4 13 33.075Q296-230 283-218l-63 63q-13.304 14-32.652 14T155-155.391ZM480-246q-98 0-166-68t-68-166q0-98 68-166t166-68q98 0 166 68t68 166q0 98-68 166t-166 68Z"
              />
            </svg>

            <svg
              v-if="
                weatherData.weather[0].icon === '02d' ||
                weatherData.weather[0].icon === '02n'
              "
              xmlns="http://www.w3.org/2000/svg"
              height="64"
              viewBox="0 -960 960 960"
              width="64"
            >
              <path
                d="m743-155-63-63q-14-12.067-14-32.033Q666-270 680-283q12-13 31.467-13 19.466 0 32.533 13l63 63q13 13.067 13 33.033Q820-167 806.947-155q-13.052 14-33 14Q754-141 743-155Zm-503-7q-65.75 0-111.875-46.177T82-320.177Q82-385 128.625-431T240-477q43.857 0 81.643 23.907T376-390l13.235 30H419q41 0 70 29t29 71q0 41-29 69.5T420-162H240Zm334-108q-5-60-47-102t-100-46q-20-53-67.5-85.5T254-536q15-79 79.5-128.5T480-714q98 0 166 68t68 166q0 70-38.5 127T574-270Zm232-165q-18.75 0-31.875-13.175-13.125-13.176-13.125-32Q761-499 774.125-512.5 787.25-526 806-526h90q18.75 0 32.375 13.675Q942-498.649 942-479.825 942-461 928.375-448T896-435h-90ZM219-679l-64-64q-14-12-14-31t14-32q11.8-13 31.4-13 19.6 0 31.6 13l64 64q13 12.8 13 31.9 0 19.1-13 31.1-13 14-32 14t-31-14Zm460 0q-14-12-14-31.467 0-19.466 14-32.533l63-63q11.067-13 31.033-13Q793-819 806-806t13 32q0 19-13 31l-64 64q-13 14-32 14t-31-14Zm-199.175-82Q461-761 448-774.125T435-806v-90q0-18.75 13.175-32.375 13.176-13.625 32-13.625Q499-942 512.5-928.375 526-914.75 526-896v90q0 18.75-13.675 31.875Q498.649-761 479.825-761Z"
              />
            </svg>

            <svg
              v-if="
                weatherData.weather[0].icon === '03d' ||
                weatherData.weather[0].icon === '03n'
              "
              xmlns="http://www.w3.org/2000/svg"
              height="64"
              viewBox="0 -960 960 960"
              width="64"
            >
              <path
                d="M253-138q-94.936 0-163.468-67.224Q21-272.449 21-370.495q0-81.435 51-147.47T204-597q22-100 100.051-162.5t178.26-62.5Q597-822 678.5-739.5T767-541v24q75 8 123.5 60.706Q939-403.587 939-327q0 79.083-54.958 134.042Q829.083-138 750-138H253Z"
              />
            </svg>

            <svg
              v-if="
                weatherData.weather[0].icon === '04d' ||
                weatherData.weather[0].icon === '04n'
              "
              xmlns="http://www.w3.org/2000/svg"
              height="64"
              viewBox="0 -960 960 960"
              width="64"
            >
              <path
                d="M737-178q-20.5 0-34.75-13.75T688-227q0-21.5 14.25-35.25T737-276q22.5 0 35.75 13.75T786-227q0 21.5-13.25 35.25T737-178ZM272.053-36Q251-36 237-49.25 223-62.5 223-84t13.75-35.25Q250.5-133 272-133q21.075 0 34.538 13.75Q320-105.5 320-84t-13.447 34.75Q293.105-36 272.053-36ZM240-178q-21.5 0-35.25-13.75T191-227q0-21.5 13.75-35.25T240-276h360q22.5 0 35.75 13.75T649-227q0 21.5-13.25 35.25T600-178H240ZM409-36q-21.5 0-35.25-13.25T360-84q0-21.5 13.75-35.25T409-133h280q22.075 0 35.037 13.75Q737-105.5 737-84t-12.963 34.75Q711.075-36 689-36H409ZM290-335q-92 0-158.5-66.5T65-560q0-84 58-149t144-74q35-57 90.5-90.5T479.962-907Q572-907 637-849q65 58 84 145 82 7 128.5 60T896-520q0 77-54.208 131Q787.583-335 710-335H290Z"
              />
            </svg>

            <svg
              v-if="
                weatherData.weather[0].icon === '09d' ||
                weatherData.weather[0].icon === '09n'
              "
              xmlns="http://www.w3.org/2000/svg"
              height="64"
              viewBox="0 -960 960 960"
              width="64"
            >
              <path
                d="M240-170q-21.5 0-35.25-13.75T191-218.5q0-22 13.75-35.75T240-268q22.5 0 35.75 13.812Q289-240.375 289-219q0 21.5-13.25 35.25T240-170Zm480 0q-21.5 0-35.25-13.75T671-218.5q0-22 13.75-35.75T720-268q22.5 0 35.75 13.812Q769-240.375 769-219q0 21.5-13.25 35.25T720-170ZM360-16q-21.5 0-35.25-13.75T311-64.5q0-22 13.75-35.75T360-114q22.5 0 35.75 13.812Q409-86.375 409-65q0 21.5-13.25 35.25T360-16Zm120-154q-21.5 0-35.25-13.75T431-218.5q0-22 13.75-35.75T480-268q22.5 0 35.75 13.812Q529-240.375 529-219q0 21.5-13.25 35.25T480-170ZM600-16q-21.5 0-35.25-13.75T551-64.5q0-22 13.75-35.75T600-114q22.5 0 35.75 13.812Q649-86.375 649-65q0 21.5-13.25 35.25T600-16ZM290-335q-91.518 0-158.259-66.655Q65-468.309 65-560q0-83.594 58-148.797T267-783q35.358-57 90.634-90.5T480.228-907q91.432 0 156.602 58T721-704q82 7 128.5 59.998T896-520.488q0 77.155-54.125 131.321Q787.75-335 710-335H290Z"
              />
            </svg>

            <svg
              v-if="
                weatherData.weather[0].icon === '10d' ||
                weatherData.weather[0].icon === '10n'
              "
              xmlns="http://www.w3.org/2000/svg"
              height="64"
              viewBox="0 -960 960 960"
              width="64"
            >
              <path
                d="m566-69-37 37q-6.909 6-17.455 6Q501-26 496-32l-35.947-36.667q-7.053-6-7.053-16.666Q453-96 460-102l36-37q5-6 15.667-6 10.666 0 17.666 6.053L566-102q5 6 5 16.546Q571-74.91 566-69Zm-238-33q-10-10.545-10-25.773Q318-143 328-155l76-76q9.818-10 26.409-10T456-231q9 12 9.5 27t-9.5 26l-77 77q-10 11-25.5 10T328-102Zm358-87-37 37q-6.909 6-17.455 6Q621-146 616-152l-35.947-36.667q-7.053-6-7.053-16.666Q573-216 580-222l36-37q5-6 15.667-6 10.666 0 17.666 6.053L686-222q5 6 5 16.545 0 10.546-5 16.455Zm-401.053.333L248-152q-6.636 6-16.818 6Q221-146 215-152l-37-37q-6-5.909-6-16.455Q172-216 178-222l36.947-36.947Q221-265 231.182-265q10.182 0 16.818 6l37 37q6 6 6 16.667 0 10.666-6.053 16.666ZM290-335q-92 0-158.5-66.655Q65-468.309 65-560q0-83.594 58-148.797T267-783q35-57 90.39-90.5T480.46-907q91.2 0 156.37 58T721-704q82 7 128.5 60.026T896-520q0 76.667-54.167 130.833Q787.667-335 710-335H290Z"
              />
            </svg>

            <svg
              v-if="
                weatherData.weather[0].icon === '11d' ||
                weatherData.weather[0].icon === '11n'
              "
              xmlns="http://www.w3.org/2000/svg"
              height="64"
              viewBox="0 -960 960 960"
              width="64"
            >
              <path
                d="m571-106-38-20q-15-7-18.5-24.5T522-181l73-85q4-7 13-11t20-4q27 0 37.5 24t-6.5 45l-33 38 38 19q15 8 19 25.5T674-99l-72 85q-5 7-14 10.5T571 0q-29 0-40-23.5t7-45.5l33-37Zm-275 0-37-20q-16-7-19.5-24.5T248-181l73-85q7-7 17-11t14-4q28 0 39 24t-7 45l-31 38 38 19q13 8 16.5 25.5T401-99l-73 85q-8 7-18 10.5T295 0q-28 0-38.5-23.5T264-69l32-37Zm-6-229q-92 0-158.5-66.5T65-560q0-84 58-149t144-74q35-57 90.5-90.5T480-907q92 0 157 58t84 145q82 7 128.5 60T896-520q0 77-54 131t-132 54H290Z"
              />
            </svg>

            <svg
              v-if="
                weatherData.weather[0].icon === '13d' ||
                weatherData.weather[0].icon === '13n'
              "
              xmlns="http://www.w3.org/2000/svg"
              height="64"
              viewBox="0 -960 960 960"
              width="64"
            >
              <path
                d="M441-252 328-140q-11 12-28.5 11.5T272-140q-12-10-12-27.5t12-29.5l169-168v-76h-76L200-276q-11 12-28.5 12T144-275q-12-10-12-27.5t12-28.5l107-110H110q-17 0-28-11.5T71-481q0-15 11.5-27t28.5-12h140L140-632q-12-10-12-28t12-29q10-10 27.5-10t29.5 10l168 169h76v-77L276-760q-12-11-11.5-28.5T276-817q10-11 27.5-11t28.5 12l109 107v-141q0-16 11.5-27.5T481-889q15 0 27 11.5t12 27.5v141l112-112q10-11 28-10.5t29 10.5q9 12 9 29t-10 29L520-597v77h77l164-165q11-11 28.5-10.5T818-685q11 11 11 28.5T817-629L710-520h140q16 0 27.5 12t11.5 27q0 17-11.5 28.5T850-441H710l109 113q12 11 11.5 28T820-273q-13 13-29 12.5T764-272L597-441h-77v76l169 170q10 11 10 28t-11 27q-11 12-28 12.5T633-139L520-252v142q0 17-12 28t-27 11q-17 0-28.5-11.5T441-111v-141Z"
              />
            </svg>

            <svg
              v-if="
                weatherData.weather[0].icon === '50d' ||
                weatherData.weather[0].icon === '50n'
              "
              xmlns="http://www.w3.org/2000/svg"
              height="64"
              viewBox="0 -960 960 960"
              width="64"
            >
              <path
                d="M160-195q-18.375 0-31.688-13.358Q115-221.716 115-240.158t13.312-32.142Q141.625-286 160-286h400q18.8 0 32.4 13.65 13.6 13.651 13.6 32.525 0 18.45-13.6 31.637Q578.8-195 560-195H160Zm560 0q-18.375 0-31.688-13.358Q675-221.716 675-240.158t13.312-32.142Q701.625-286 720-286h80q18.8 0 32.4 13.65 13.6 13.651 13.6 32.525 0 18.45-13.6 31.637Q818.8-195 800-195h-80ZM160-355q-18.375 0-31.688-13.358Q115-381.716 115-400.158t13.312-32.142Q141.625-446 160-446h80q18.8 0 32.4 13.65 13.6 13.651 13.6 32.525 0 18.45-13.6 31.637Q258.8-355 240-355h-80Zm240 0q-18.375 0-31.688-13.358Q355-381.716 355-400.158t13.312-32.142Q381.625-446 400-446h400q18.8 0 32.4 13.65 13.6 13.651 13.6 32.525 0 18.45-13.6 31.637Q818.8-355 800-355H400ZM160-515q-18.375 0-31.688-13.358Q115-541.716 115-560.158t13.312-32.142Q141.625-606 160-606h440q18.8 0 32.4 13.65 13.6 13.651 13.6 32.525 0 18.45-13.6 31.637Q618.8-515 600-515H160Zm600 0q-18.375 0-31.688-13.358Q715-541.716 715-560.158t13.312-32.142Q741.625-606 760-606h40q18.8 0 32.4 13.65 13.6 13.651 13.6 32.525 0 18.45-13.6 31.637Q818.8-515 800-515h-40ZM160-675q-18.375 0-31.688-13.358Q115-701.716 115-720.158t13.312-32.142Q141.625-766 160-766h200q18.8 0 32.4 13.65 13.6 13.651 13.6 32.525 0 18.45-13.6 31.637Q378.8-675 360-675H160Zm360 0q-18.375 0-31.688-13.358Q475-701.716 475-720.158t13.312-32.142Q501.625-766 520-766h280q18.8 0 32.4 13.65 13.6 13.651 13.6 32.525 0 18.45-13.6 31.637Q818.8-675 800-675H520Z"
              />
            </svg>
            <p>{{ weatherData.weather[0].description }}</p>
          </div>
          <h1>{{ Math.round(weatherData.main.temp) }}<span>°C</span></h1>
        </main>
      </div>
      <div class="ion-padding details">
        <ul>
          <li>
            Wind
            <span
              ><b>{{ weatherData.wind.speed }}</b> m/s</span
            >
          </li>
          <li>
            Humidity
            <span
              ><b>{{ weatherData.main.humidity }}</b
              >%</span
            >
          </li>
          <li>
            Pressure
            <span
              ><b>{{ weatherData.main.pressure }}</b> hPa</span
            >
          </li>
        </ul>
      </div>
    </ion-content>
    <div class="hide" v-else>
      <div class="ion-padding enf" v-if="!loadData">
        <h2>Something is <b>wrong</b>.<br />Please try again later.</h2>
        <img src="../assets/512.webp" />
      </div>
    </div>
  </ion-app>
</template>

<script lang="ts">
import { IonApp, IonContent, IonSearchbar, IonChip } from "@ionic/vue";
import { defineComponent } from "vue";
import { StatusBar, Style } from "@capacitor/status-bar";
import { Capacitor } from "@capacitor/core";
import axios from "axios";

interface WeatherData {
  name: string;
  main: {
    temp: number;
    humidity: number;
    pressure: number;
  };
  weather: {
    icon: string;
    main: string;
    description: string;
  }[];
  wind: {
    speed: number;
  };
}

if (Capacitor.getPlatform() === "android") {
  StatusBar.setOverlaysWebView({ overlay: true });
  StatusBar.setStyle({ style: Style.Dark });
}

async function fetchWeatherData(cityName: string): Promise<WeatherData> {
  try {
    const apiKey = "IS_HIDDEN";
    const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${cityName}&units=metric&appid=${apiKey}`;
    const response = await axios.get(apiUrl);
    return response.data;
  } catch (error) {
    console.error("Error :", error);
    throw error;
  }
}

export default defineComponent({
  components: { IonApp, IonContent, IonSearchbar, IonChip },

  data() {
    return {
      currentDate: new Date(),
      weatherData: null as WeatherData | null,
      cityName: "Manado",
      loadData: false,
      cityNotFound: false,
    };
  },
  mounted() {
    this.fetchWeather();
  },
  methods: {
    async fetchWeather() {
      try {
        this.loadData = true;
        const weatherData = await fetchWeatherData(this.cityName);
        this.weatherData = weatherData;
        this.cityNotFound = false;
      } catch (error) {
        console.error("Error :", error);
        this.cityNotFound = true;
        throw error;
      } finally {
        this.loadData = false;
      }
    },
    updateCityName(event: any) {
      const inputText = event.target.value;
      this.cityName = inputText.trim() === "" ? "Manado" : inputText;
      this.fetchWeather();
    },
    formatDate(date: any) {
      const options = {
        weekday: "long",
        year: "numeric",
        month: "short",
        day: "numeric",
      };
      return date.toLocaleDateString("en-US", options);
    },
  },
});
</script>

<style scoped>
ion-searchbar {
  --border-radius: 999px;
  --box-shadow: none;
  margin-top: 1em;
}

.we-are-center {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  min-height: 50vh;
}

.start,
.current-weather,
.enf {
  display: grid;
  place-items: center;
}

.current-weather p {
  text-transform: capitalize;
}

.start {
  margin-top: 0.5em;
}

.current-weather svg {
  fill: var(--color-svg);
}

ion-chip {
  padding: 1.5em 1.4em 1.5em 1em;
  background-color: var(--color-yellow);
  border-radius: 999px;
  font-size: 0.875rem;
}

ion-chip p {
  margin-left: 0.5em;
  color: #000;
}

h1 {
  font-size: 6rem;
  font-weight: bold;
  margin: 0;
  margin-top: -0.1em;
}

h1 span {
  font-size: 3rem;
  font-weight: normal;
}

.details {
  position: fixed;
  left: 0;
  bottom: 0;
  min-width: 100vw;
}

.details ul {
  list-style: none;
  padding: 0 1em;
}

.details ul li {
  display: flex;
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
  justify-content: space-between;
  align-items: center;
  padding-bottom: 0.8em;
  margin-bottom: 1em;
}

.enf {
  min-height: 100vh;
  color: #fff;
}
</style>
