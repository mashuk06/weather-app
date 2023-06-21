<script>
    import { ref, onMounted } from 'vue'
    import axios from 'axios'

    export default {
        setup() {
            const searchQuery = ref('Dhaka');
            const location = ref({});
            const current = ref({});

            const getWeatherData = () => {
                let apiUrl = 'http://api.weatherapi.com/v1/current.json?key=61da88ac6ba7489c9f4154237232106';
                if (searchQuery.value) {
                    apiUrl += `&q=${searchQuery.value}&aqi=no`;
                }

                axios.get(apiUrl)
                    .then(response => {
                        const { location: loc, current: curr } = response.data;
                        console.log(response.data)
                        location.value = loc;
                        current.value = curr;
                    })
                    .catch(error => {
                        console.error(error);
                    });
            };

            onMounted(() => {
                getWeatherData();
            });

            const getIconUrl = (icon) => {
                return `https:${icon}`;
            };

            return {
                searchQuery,
                getWeatherData,
                location,
                current,
                getIconUrl
            };
        }
    }
</script>

<template>
    <section class="py-2 vh-100 vw-50" style="background-color: #454647;">
        <h2 class="text-warning">Weather App</h2>
        <hr class="text-warning">
        <div class="container">
            <div class="search-container py-1">
                <input v-model="searchQuery" class="search-input rounded-pill" type="text"
                    placeholder="Enter city name/Zip code">
                <button class="search-button rounded-pill mx-3" @click="getWeatherData">
                    Search
                </button>
            </div>
            <div class="h-100 w-100 py-5 px-10 h-100">
                <div class="row d-flex justify-content-center align-items-center">
                    <div>
                        <div class="card" style="color: #4B515D; border-radius: 35px;">
                            <div class="card-body p-4">

                                <div class="text-left text-success">
                                    <h3 class="flex-grow-1">{{ location.name }}, {{ location.country }}</h3>
                                </div>
                                <div class="text-left text-dark">
                                    <h6>Time: {{ location.localtime }}</h6>
                                </div>
                                <div class="d-flex flex-column text-center mt-5 mb-4">
                                    <h6 class="display-4 mb-0 font-weight-bold" style="color: #1C2331;"> {{
                                        current.temp_c }}°C / {{ current.temp_f }}°F </h6>
                                    <span v-if="current && current.condition" class="small" style="color: #868B94">{{
                                        current.condition.text }}</span>
                                </div>

                                <div class="d-flex align-items-center">
                                    <div class="flex-grow-1" style="font-size: 1rem;">
                                        <div><i class="fas fa-wind fa-fw" style="color: #868B94;"></i> <span
                                                class="ms-1">
                                                Wind: {{ current.wind_kph }} km/h from {{ current.wind_dir }}
                                            </span></div>
                                        <div><i class="fas fa-tint fa-fw" style="color: #868B94;"></i> <span
                                                class="ms-1">
                                                Pressure: {{ current.pressure_mb }} mb </span>
                                        </div>
                                        <div><i class="fas fa-sun fa-fw" style="color: #868B94;"></i> <span
                                                class="ms-1">
                                                Humidity: {{ current.humidity }}% </span>
                                        </div>
                                        <div><i class="fas fa-sun fa-fw" style="color: #868B94;"></i> <span
                                                class="ms-1">
                                                Visibility: {{ current.vis_km }} km </span>
                                        </div>
                                    </div>
                                    <div>
                                        <img v-if="current && current.condition"
                                            :src="getIconUrl(current.condition.icon)" :alt="current.condition.text">
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<style scoped>
    .search-container {
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .search-input {
        padding: 8px 12px;
        border: 1px solid #ced4da;
        border-radius: 999px;
        outline: none;
    }

    .search-button {
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 8px;
        border: none;
        background-color: #79b7f5;
        color: #fff;
        cursor: pointer;
    }

    .search-button i {
        font-size: 1.2rem;
    }
</style>
