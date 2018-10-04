<template>
  <div id="app">
    
<section class="hero navImage">
  <div class="hero-body">
    <div class="container">
      <h1 class="title is-size-2 has-text-centered has-text-white">
        {{ title }}
      </h1>
    </div>
  </div>
</section>

<div class="container">


<section class="section input-section">
<div class="field">
  
  <label>Please Enter City Name: </label>
    <input class="input is-info is-medium is-rounded" id="city" type="text" placeholder="Enter US City Name" autofocus maxlength="20" required><br><br>
    <button class="button is-info is-small" @click="weather">Get Current Weather For City</button>
    <br><br>
 


  
  <label>Please Enter Zip Code: </label>
    <input class="input is-info is-medium is-rounded" id="zip"  type="text" placeholder="Enter 5 Digit ZipCode" autofocus maxlength="5" required><br><br>
    <button class="button is-info is-small" @click="weather2">Get Current Weather By Zip</button>
    <br><br>
    <button class="button is-info is-small" @click="get5day">Get 5Day</button>
  
</div>
</section>
<section class="section info-section has-text-centered">

  <div class="columns">

    <h1 class='title'>{{ city }}<br></h1>
    
    <img class="image row" src="./static/images/degreeF.png">{{ tempF }}F<br>
    {{ tempC }}C<br>
    <img class="image row" src="./static/images/humidity.png">{{ humidity }}<br>
    <img class="image row" src="./static/images/current.png">{{ currWeather }}<br>

  </div>

</section>
  


<section>
<div class="card">

    <ul>
      <!-- For each unique date returned from the API, we need to find the correct data that matches that data -->
      <li v-for="date in uniqueDates">
        <!-- Print out that unique date -->
        <p class="is-size-3">{{formatDate(date)}}<br></p>
        <!-- Now we need to look in our dataArray, which contains an array for each date with that dates weather information -->
        <div v-for="array in dataArray">
          <!-- As we look through each array in our dataArray, we use a function to check if the data in that array is from our unique date, and if so print it under that unique data -->
          <div v-for="data in dateMatches(array, date)">
            <h1 class='title is-size-4'>
              <p class="infoText">
                {{formatTime(data.dt_txt)}}<br>
                </p>
              </h1>
              <div class="content">
            <figure class="image"><img src="./static/images/degreeF.png"></figure> 
            <p class="infoText2">{{tempConversionF(data.main.temp)}}</p>
            <p class="infoText2">{{tempConversionC(data.main.temp)}}</p>
            <figure class="image"><img src="./static/images/humidity.png"></figure>
             <p class="infoText2">{{data.main.humidity}}</p>
            <figure class="image"><img src="./static/images/current.png"></figure> 
            <p class="infoText2">{{data.weather[0].description}}</p>
            <hr>
              </div>
          </div>
        </div>
        
      </li>
    </ul>

</div>

</section>

 
  </div>
</div>

</template>

<script>
import moment from 'moment';

export default {
  name: 'app',
  data () {
    return {
      title: 'Weather App',
      state: '',
      city: '',
      day: '',
      days: '',
      tempF: '',
      tempC: '',
      humidity: '',
      currWeather: '',
      uniqueDates: [],
      dataArray: []
      
    }
  },

   methods: {

    formatDate(date) {
      return moment(date).format(`dddd MMMM D, gggg`)

    },
    formatTime(time) {
      return moment(time).format(`hh:mm A`)

    },
    tempConversionF(temp) {
      return Math.round((temp * 9/5) - 459.67) + ' F'

    },
    tempConversionC(temp) {
      return Math.round(temp - 273.15) + ' C'

    },
    
    dateMatches: function(data, checkDate) {
     return data.filter(function(date) {
       let split = date.dt_txt.split(" ")
       return split[0] === checkDate
     })
   },

   getWeather() {
       let city = document.getElementById('city').value
       let url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&?units=Fahrenheit&APPID=09e5878249822dd2c44cdf6cc6e3fa7b`;
       axios
         .get(url)
         .then(response => {
           console.log(response)
           this.tempF = Math.round((response.data.main.temp * 9/5) - 459.67)
           this.tempC = Math.round(response.data.main.temp - 273.15)
           this.city = response.data.name
           this.humidity = response.data.main.humidity
           this.currWeather = response.data.weather[0].description
       })
       .catch(error => {
         console.log(error);
       });
     },
     getWeather2() {
       let zip = document.getElementById('zip').value
       let url = `https://api.openweathermap.org/data/2.5/forecast?zip=${zip}&APPID=09e5878249822dd2c44cdf6cc6e3fa7b`;
       let day = ''
        // axios call to OpenWeatherApp API to retrieve current forecast by ZIP code
       axios
         .get(url)
         .then(response => {
           console.log(response)
           this.city = response.data.city.name
           this.tempF = Math.round((response.data.list[0].main.temp * 9/5) - 459.67)
           this.tempC = Math.round(response.data.list[0].main.temp - 273.15) 
           this.humidity = response.data.list[0].main.humidity
           this.currWeather = response.data.list[0].weather[0].description 
           day = response.data.list[0].dt_txt
           this.day = moment(day).format(`dddd MMMM gggg h:mm a`)
          
           
       })
       .catch(error => {
         console.log(error);
       });
     },

     get5day() {
       let zip = document.getElementById('zip').value
       let url = `https://api.openweathermap.org/data/2.5/forecast?zip=${zip}&APPID=09e5878249822dd2c44cdf6cc6e3fa7b`;
       let forecast = ''
       // axios call to OpenWeatherApp API to retrieve 5day forecast by ZIP code
       axios
         .get(url)
         .then(response => {
          //  console.log(response)
            forecast = response.data.list
            let allDates = []
            forecast.forEach((d) => {
              let x = d.dt_txt
              // splitting the date into month-year-day and hours
              let y = x.split(" ")
              // taking just the month year day from the split date
              let z = y[0]
              //array that contains all dates returned
              allDates.push(z)
             
              this.city = response.data.city.name
              this.day = moment(x).format(`dddd MMMM gggg h:mm a`)
            })
            console.log(allDates)
            //using filter on list of all dates to return an array of unique dates
            let uniqueDates = allDates.filter((value, index, self)=> {
              return self.indexOf(value) === index
            })
            //store uniqueDates in our state for later use
            this.uniqueDates = uniqueDates
            console.log(uniqueDates)
            //array that will contain an array for each day
            let dataArray = []
            //for each unique date, look through the full data returned from API
            uniqueDates.forEach((d) => {
              let dayArray = []
              //check each observation returned by the API to see which unique date it matches, and add it to that dates array of data
              forecast.forEach((d2) => {
                let x = d2.dt_txt
                let y = x.split(" ")
                if(y[0] === d) {
                  dayArray.push(d2)
                  
                }
              })
              dataArray.push(dayArray)
            })
            this.dataArray = dataArray
            
         
           
       })
       .catch(error => {
         console.log(error);
       });
     },
  
     weather() {
    
    this.getWeather()
  },
     weather2() {
    
    this.getWeather2()
  },
  }
};
</script>

<style>
body{
  font-family: 'Nunito', sans-serif;
}

.infoText {
  font-weight: bold;
  text-transform: uppercase;
 
}

.infoText2 {
  text-transform: uppercase;
}

.card {
  padding: 10px;
  width: 100%;
  
}



.navImage {
  background: url(./static/images/nav.jpg) no-repeat center center scroll;
  background-size: cover;
  
  }
.image {
  width: 40px;
  height: 40px;
  
}

.info-section {
  margin-top: -75px;
}

.input-section {
  margin-top: -25px;
}

</style>
