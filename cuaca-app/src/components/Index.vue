<template>

    <div class="container">

        <div class="weather-card">

            <!-- header -->
             <div class="header">

                <img 
                    src="../assets/weather.png" alt="Weather Logo" class="logo"  
                />

                <div>
                    <h1>Cuaca App</h1>
                    <p>Prakiraan suhu per jam</p>
                </div>
             </div>

             <!-- tabel -->
              <table>

                <thead>
                    <tr>
                        <th>No</th>
                        <th>Waktu</th>
                        <th>Suhu</th>
                    </tr>
                </thead>

                <tbody>
                    <tr v-for="(item, index) in weatherData" :key="index">
                        <td>{{ index + 1 }}</td>
                        <td>{{ formatDate(item.time) }}</td>
                        <td>
                            <span class="temp-badge">
                                {{ item.temperature }}°C
                            </span>
                        </td>
                    </tr>
                </tbody>
              </table>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({

    name: 'Index',

    data(){
        return{

            weatherData: [] as Array<{
                time: string,
                temperature: number
            }>
        }
    },

    methods: {
        async getWeatherData(){
            try {
                
                const response = await fetch(
                    'https://api.open-meteo.com/v1/forecast?latitude=-6.2&longitude=106.8&hourly=temperature_2m'
                )

                const data = await response.json();

                const times = data.hourly.time
                const temperatures = data.hourly.temperature_2m

                // Menampilkan 10 data Pertama
                for (let i = 0; i < 10; i++) {
                    this.weatherData.push({
                        time: times[i],
                        temperature: temperatures[i]
                    })
                }
            } catch (error) {
                console.error('Error fetching weather data:', error);
        }
    },

    formatDate(dateString: string): string {
        
        return new Date(dateString).toLocaleTimeString('id-ID')
    }

    },

    mounted() {
        this.getWeatherData()
    }

})
</script>

<style scoped>

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Poppins', sans-serif;
}

.container {
    
    min-height: 100vh;

    background: linear-gradient(
        135deg,
        #4facfe,
        #00f2fe
    );

    display: flex;

    justify-content: center;
    align-items: center;

    padding: 20px;

}

.weather-card {

    width: 100%;
    max-width: 900px;

    background: white;

    padding: 30px;

    border-radius: 25px;

    box-shadow: 0 10px 30px rgba(0,0,0,0.2);
}

.header {
    display: flex;
    align-items: center;
    gap: 15px;

    margin-bottom: 25px;
}

.logo {
    width: 70px;
    height: 70px;
}

h1 {

    color: #2c3e50;
    font-size: 32px;
}

p {
    color: #666;
    margin-top: 5px;
}

table {
    width: 100%;
    border-collapse: collapse;

    overflow: hidden;
}

thead {
    background: #3498db;
    color: white;
}

th, td {
    padding: 15px;
    text-align: center;

    border-bottom: 1px solid #ddd;
}

tbody tr:nth-child(even) {
    background: #f8fbff;
}

tbody tr:hover {
    background: #eaf6ff;

    transition: 0.3s;
}

.temp-badge {
   
    background: #3498db;
    color: white;

    padding: 8px 14px;

    border-radius: 20px;

    font-weight: bold;
}

@media (max-width: 600px) {
    .weather-card {
        padding: 20px;
    }

    .header {
        flex-direction: column;
        text-align: center;
    }

    .logo {
        width: 60px;
        height: 60px;
    }

    h1 {
        font-size: 24px;
    }

    th,
    td {
        font-size: 12px;
        padding: 10px;
    }
}
</style>