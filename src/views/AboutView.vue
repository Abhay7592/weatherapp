<template>
  <div class="about">
    <h1>This is an about page</h1>
    <input type="text" placeholder="Enter the city name" v-model="city">
    <input type="button" value="Submit" @click="getTemperature">
    <div id="temp">
      <p v-if="temperature">The temperature in {{ city }} is {{ temperature }}°C.</p>
      <p v-if="!temperature && submitted">Failed to retrieve temperature for {{ city }}.</p>

    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      city: '',
      temperature: null,
      submitted: false
    }
  },
  methods: {
    async getTemperature() {
      const apiKey = '11b298b30122c6fe932356e5742000db';
      const url = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=${apiKey}`;

      console.log('Fetching temperature for', this.city);
      this.submitted = true;

      try {
        const response = await fetch(url);
        const data = await response.json();
        console.log('API response:', data);

        if (data.main) {
          this.temperature = data.main.temp;
          console.log('Temperature:', this.temperature);
        } else {
          this.temperature = null;
        }
      } catch (error) {
        console.error(error);
        this.temperature = null;
      }
    }
  }
}
</script>

<style>
.about {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}

.about h1 {
  font-size: 24px;
  margin-bottom: 20px;
}

.about input[type="text"] {
  display: block;
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.about input[type="button"] {
  display: block;
  width: 100%;
  padding: 10px;
  margin-top: 10px;
  font-size: 16px;
  color: #fff;
  background-color: #007bff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.about p {
  margin-top: 10px;
  font-size: 16px;
  color: #333;
}

</style>
