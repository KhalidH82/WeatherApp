# WeatherApp
<a href="https://khalidh82.github.io/WeatherApp/">

> A Vue.js project

<h3>## Build Setup</h3>
*Make sure to have Vue cli installed globally npm install -g vue-cli
1. Clone or download to local machine
2. CD into directory
<h3># install dependencies</h3>
3. Run npm install in terminal projects root directory
<h3># serve with hot reload at localhost:8080</h3>
4. Run npm run dev

<h4>Description</h4>
<hr>
<p>When creating this weather application using the data retrieved from the OpenWeatherMap forcast API, was to use a dynamic front end Javascript framework Vue.js. Since this was a single page application to display weather information I could have my HTML, JS, and CSS code in one .vue file. Once obtaining my API key I decided to use Axios to call the API and return the data I requested. Once I was targeting the correct data based on the parameters sent for the request I was able to display the results of the search based on the city name as well as the zip code. To get the data for the 5 day forcast was challenging because of the amount of data that was recieved. I had to aggregate the data down into specific dates with the relative information I was looking for. Another issue I had to work around was converting the dates into a readable format for the user. For that I used Moment.js which is a npm package that formats dates and times into a readable format. Some tradeoffs that I made was displaying the current weather based on geolocation which would load as soon as user visits page. Unfortunatly, that was creating issues my other API calls so I decided to leave that feature out. Some features that I would like to have added with more time is an improved UI. I would like to have added a drop down menu so the user can select between city search or zip search which would display only information specific to that search. I would also like to include the current weather based on the geolocation. Also, I would like to have created some additional components to render out the information in a more visually appealing way. I have used a global packagage manager to install Vue CLI and also NPM to install Moment.js.</p>

