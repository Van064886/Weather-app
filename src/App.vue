<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
        <main>
              <div class="search-box">
                    <input type="search" v-model="query" name="search" id="search" 
                           placeholder="Search........."
                           @keypress="fetchWeather"/>
              </div>
              <div class="weather-app" v-if="typeof weather.main != 'undefined'">
                    <div class="weather-location">
                          <div class="location-box">
                                <div class="location"> 
                                    {{ weather.name }},{{ weather.sys.country }}
                                </div>
                                <div class="date"> {{ dateBuilder() }}</div>
                          </div>
                          <div class="weather-box">
                                <div class="temp"> {{ Math.round( weather.main.temp ) }}°C</div>
                                <div class="weather"> 
                                     {{ weather.weather[0].main }}
                                </div>
                          </div>
                    </div>
              </div>
        </main>      
  </div>
</template>

<script> 

export default 
{
  name: 'App',
  data()
  {
      return {
          api_key: '56f4b3d21b29f9210cb958f2ebcc5de4',
          url_base: 'https://api.openweathermap.org/data/2.5/',
          query: '',
          weather:
          {

          }
      }
  }, 
  methods:
  {
      fetchWeather(e)
      {
          if ( e.key == "Enter" )
          {
              fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
                    .then( res => {
                        return res.json();
                    })
                    .then(this.setResult);
          }
      },

      setResult(result)
      {
          this.weather = result;
      },

      dateBuilder()
      {
          let d = new Date();
          let months = [ "Janvier", "Fevrier", "Mars", "Avril", "Mai", "Juin", "Juillet", "Août", 
                          "Septembre", "Octobre", "Novembre", "Décembre"
                       ];
          let days = [ "Lundi", "Mardi", "Mercredi", "Jeudi", "Vendredi", "Samedi", "Dimanche" ];

          let day = days[d.getDay()];
          let month = months[d.getMonth()];
          let year = d.getFullYear();
          let date = d.getDate();

          return `${day} ${date} ${month} ${year}`;
      }
  }
}
</script>

<style>
*
{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

body
{
    font-family: monospace;
}

#app 
{ 
    background-image: url("./assets/www.jpg");
    background-size: cover;
}

#app.warm
{
    background-image: url("./assets/wwww.jpg");
}

main
{
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0,0,0,0.25),rgba(0,0,0,0.75));
}

.search-box
{
    width: 100%;
    margin-bottom: 30px;
}
#search
{
    display: block;
    width: 100%;
    padding: 15px;
    color: #313131;
    font-size: 20px;
    border: none;
    appearance: none;
    outline: none;
    box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
    background-color: rgba(255, 255, 255, 0.9);
    border-radius: 0px 15px 0px 15px;
    transition: 0.4s;
}

#search:focus
{ 
    box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
    border-radius: 16px 0px 16px 0px;
}

.location-box .location
{
    color : #FFF;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);

}

.location-box .date
{
    color : #FFF;
    font-size: 18px;
    margin-top: 11px;
    font-weight: 300;
    font-style: italic;
    text-align: center; 
}

.weather-box
{
    text-align: center;
}

.weather-box .temp
{
    display: inline-block;
    padding: 10px 25px;
    font-size: 102px;
    color: #FFF;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.25);
    box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    border-radius: 16px;
    margin: 30px 0;
}

.weather-box .weather
{
    color: #FFF;
    font-size: 48px;
    font-weight: 700;
    font-style: italic;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

</style>
