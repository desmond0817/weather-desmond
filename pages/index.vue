<template>
    <div class="font-poppins" style="background: #F5F5FB; height: fit-content;">

        <nav class="navbar docs-navbar is-spaced has-shadow" style="background: #333; position: fixed; width: 100%;">
            <div class="container">
                <div class="navbar-brand">
                    <!-- <a href="/" class="navbar-item" title="Buefy: lightweight UI components for Vue.js based on Bulma">
                    <img src="/static/img/buefy.1d65c18.png" alt="Buefy"> 
                    </a> -->
                    <div class="has-text-weight-bold px-4 py-2 has-text-white mx-4 logo" style="border: 1px solid #fff;">Open<br> Weather</div>
                </div>
            </div>
        </nav>


    <div class="container p-4" style="height: calc(100% - 84px); padding-top: 110px !important">

        <div class="has-text-weight-bold mb-3 is-size-6-mobile is-size-5" style="color: #848484;">Location</div>

        <div class="columns d-card d-columns-fix p-4 mb-6 d-shadow">
            <div class="column">               
                <b-select v-model="city" @input="changeLocation" expanded v-if="!useCurrentLocation">
                    <option
                        v-for="option in data2"
                        :value="option"
                        :key="option">
                        {{ option }}
                    </option>
                </b-select>    
                <b-input v-model="currentLocationName" expanded v-if="useCurrentLocation"></b-input>          
            </div>

             <div class="column">
                <!-- <b-button type="is-primary" @click="getLocation">Use Current Location</b-button> -->
                <b-checkbox class="is-size-6" v-model="useCurrentLocation" @input="inputCurrentLocation" style="height: 40px;">
                    Use Current Location
                </b-checkbox>         
                <!-- <b-checkbox-button v-model="useCurrentLocation"
                    native-value="useCurrentLocation"
                    type="is-success">
                    <b-icon v-if="useCurrentLocation" icon="check"></b-icon>
                    <span>Use Current Location</span>
                </b-checkbox-button>   -->
            </div>
        </div>

        <!-- <p>Lat = {{ lat }} Lon ={{ lon }}</p>
	    <p>{{ error }}</p> -->

        <div class="has-text-weight-bold mb-3 is-size-6-mobile is-size-5" style="color: #848484;">Current Weather</div>
        <div class="columns d-columns-fix mb-6">
        <div class="column is-full-mobile is-half-widescreen p-4 d-shadow" style="border-radius: 1rem; border: 1px solid #f5f5f5; background: #fff;">  
            <!-- need to figure out -->
            <div v-for="item in weatherData.weather" :key="item.id">  
                <div class="px-2 mb-4">
                    <div class="has-text-weight-bold is-size-5">{{ weatherData.name }}</div>   
                    <div class="has-text-grey is-size-6 is-size-7-mobile">{{ getTimeFormat(weatherData.dt) }}</div> 
                </div>

                <!-- <hr class="mt-3 mb-2"/> -->

        <div style="border: 1px solid #dbdbdb; border-radius: 0.5rem;">
            <article class="media p-4" >
              <div class="media-left px-1">
                <figure class="is-hidden-mobile image is-64x64 has-text-centered" style="background: #fff; border: 1px solid #f5f5f5; border-radius: 100%; position: relative;">
                  <img :src="`http://openweathermap.org/img/wn/` + weatherData.weather[0].icon + `@2x.png`" alt="">
                </figure>
                 <figure class="is-hidden-tablet image is-48x48 has-text-centered" style="background: #fff; border: 1px solid #f5f5f5; border-radius: 100%; position: relative;">
                  <img :src="`http://openweathermap.org/img/wn/` + weatherData.weather[0].icon + `@2x.png`" alt="">
                </figure>
              </div>
              <div class="media-content pr-4">
                <div class="content is-size-6 is-size-7-mobile">

                  <div>
                    <strong class="is-size-5">{{ weatherData.weather[0].main }}</strong> 
                    <br>
                    <div class="has-text-grey">{{ weatherData.weather[0].description }}</div>
                  </div>

                </div>
               
              </div>
              <div class="media-right px-1">
                  <div class="content is-size-6 is-size-7-mobile">
                    

                  <div class="has-text-right">
                    <strong class="is-size-5">{{ Math.round(weatherData.main.temp - 273) }} °C</strong> 
                    <br>
                    <div class="has-text-grey">Feels like {{ Math.round(weatherData.main.feels_like - 273) }} °C</div>
                    
                  </div>
                  
                </div>
                  
                </div>
            </article>
              
            <hr class="my-0 mx-4" style="background: #dbdbdb; height: 1px;"/>
             <article class="columns mx-0 is-size-6 is-size-7-mobile is-multiline is-mobile pb-3 mt-2 mb-1 px-2" >
            <div class="column px-0 is-half-tablet is-full-mobile">
            <!-- dew point: Td = T - ((100 - RH)/5.) -->       
            
            <div class="columns d-columns-fix is-multiline is-mobile"><div class="column py-0 is-half-mobile has-text-grey">Wind Speed </div><div class="column py-0 is-half has-text-weight-bold">{{ weatherWindData.speed }}m/s S    </div></div>
            <div class="columns d-columns-fix is-multiline is-mobile"><div class="column py-0 is-half has-text-grey">Humidity </div><div class="column py-0 is-half has-text-weight-bold">{{ weatherMainData.humidity }}%   </div></div>
            <div class="columns d-columns-fix is-multiline is-mobile"><div class="column py-0 is-half has-text-grey">Dew Point </div><div class="column py-0 is-half has-text-weight-bold">{{ Math.round(weatherMainData.temp - 273) - ((100 - weatherMainData.humidity)/5) }} °C  </div></div>
            </div>
            <div class="column px-0 is-half-tablet is-full-mobile">
                <div class="columns d-columns-fix is-multiline is-mobile"><div class="column py-0 is-half has-text-grey">Pressure </div><div class="column py-0 is-half has-text-weight-bold">{{ weatherMainData.pressure }}kPa  </div></div>
                <div class="columns d-columns-fix is-multiline is-mobile"><div class="column py-0 is-half has-text-grey">Visibility </div><div class="column py-0 is-half has-text-weight-bold">{{ (weatherData.visibility/1000) }}km   </div></div>
            </div>
            </article>
            </div>
            </div>

        </div>
        </div>

        <div class="has-text-weight-bold mb-3 is-size-6-mobile is-size-5" style="color: #848484;">5 Day Forecast </div>
        <div class="columns d-columns-fix mb-6">
            <div class="column is-full p-4 d-shadow" style="border-radius: 1rem; border: 1px solid #f5f5f5; background: #fff; overflow-x: scroll;"> 
                <div style="width: 4000px;">
                <div v-for="item in forecastData.list" :key="item.id" class="is-inline-block has-text-centered" >  
                    <div class="px-4 is-size-6 is-size-7-mobile" style="width: 100px;">{{  getForecastTimeFormat(item.dt) }}</div>
                    <div class="has-text-centered">
                        <figure class="image is-64x64 has-text-centered" style="margin-left: auto; margin-right: auto;">
                            <img :src="`http://openweathermap.org/img/wn/` + item.weather[0].icon + `@2x.png`"  alt="" >
                        </figure>
                    </div>
                    <div class="px-4 is-size-6 is-size-7-mobile" style="width: 100px;">{{  item.weather[0].main }}</div>
                     <div class="px-4 is-size-6 is-size-7-mobile" style="width: 100px;">{{  Math.round(item.main.temp - 273) }} °C</div>

                </div>
                </div>
            </div>
        </div> 

        <div class="mb-3 is-flex">
        <div class="has-text-weight-bold is-size-6-mobile is-size-5" style="color: #848484;">
            Watchlist
            </div>
            <b-select v-model="selectCarousel" size="is-small" style="margin-left: auto;">
                <option
                    v-for="option in data3"
                    :value="option"
                    :key="option">
                    {{ option }}
                </option>
            </b-select> 
            <b-button class="ml-3" size="is-small" type="" icon-right="plus" @click="addNewItemToCarousel(selectCarousel)"/>
        </div>
        <div class="mb-6">
            <!-- desktop version -->
            <b-carousel-list class="is-hidden-mobile is-hidden-tablet-only" v-model="test" :data="carouselItems" :items-to-show="3" :arrow="true" :arrow-hover="false">
                <template #item="list">
                    <div class="card" style="border-radius: 1rem; height: 100%;">
                        <div class="card-content">
                            <div class="content">
                                <p class="title is-6 mb-0">{{ list.location }}</p>
                                <div class="has-text-grey is-size-7">{{  getTimeFormat(list.time) }}</div>

                                <hr class="my-4" />

                                <article class="media" >
                                    <div class="media-left px-1" style="width: 48px;">
                                        <figure class="image is-48x48 has-text-centered mx-0" style="background: #fff; border: 1px solid #f5f5f5; border-radius: 100%;">
                                            <img :src="`http://openweathermap.org/img/wn/` + list.icon + `@2x.png`" alt="">
                                        </figure>
                                    </div>
                                    <div class="media-content pr-4">
                                        <div class="content is-size-7">
                                            <div>
                                                <strong class="is-size-6">{{ list.main }}</strong> 
                                                <br>
                                                <div class="has-text-grey">{{ list.description }}</div>
                                            </div>
                                            
                                        </div>
                                    </div>
                                    <div class="media-right px-1">
                                        <div class="content is-size-7">
                                            <div class="has-text-right">
                                                <strong class="is-size-6">{{ Math.round(list.temp - 273) }} °C</strong> 
                                                <br>
                                                <div class="has-text-grey">Feels like {{ Math.round(list.feelsLike - 273) }} °C</div>
                                                
                                            </div>
                                            
                                        </div>
                                    </div>
                                </article>
                                
                                
                                
                            </div>
                        </div>
                    </div>
                </template>
            </b-carousel-list> 

            <!-- tablet version -->
            <b-carousel-list class="is-hidden-mobile is-hidden-desktop" v-model="test" :data="carouselItems" :items-to-show="2" :arrow="true" :arrow-hover="false">
                <template #item="list">
                    <div class="card" style="border-radius: 1rem; height: 100%;">
                        <div class="card-content">
                            <div class="content">
                                <p class="title is-6 mb-0">{{ list.location }}</p>
                                <div class="has-text-grey is-size-7">{{  getTimeFormat(list.time) }}</div>

                                <hr class="my-4" />

                                <article class="media" >
                                    <div class="media-left px-1" style="width: 48px;">
                                        <figure class="image is-48x48 has-text-centered mx-0" style="background: #fff; border: 1px solid #f5f5f5; border-radius: 100%;">
                                            <img :src="`http://openweathermap.org/img/wn/` + list.icon + `@2x.png`" alt="">
                                        </figure>
                                    </div>
                                    <div class="media-content pr-4">
                                        <div class="content is-size-7">
                                            <div>
                                                <strong class="is-size-6">{{ list.main }}</strong> 
                                                <br>
                                                <div class="has-text-grey">{{ list.description }}</div>
                                            </div>
                                            
                                        </div>
                                    </div>
                                    <div class="media-right px-1">
                                        <div class="content is-size-7">
                                            <div class="has-text-right">
                                                <strong class="is-size-6">{{ Math.round(list.temp - 273) }} °C</strong> 
                                                <br>
                                                <div class="has-text-grey">Feels like {{ Math.round(list.feelsLike - 273) }} °C</div>
                                                
                                            </div>
                                            
                                        </div>
                                    </div>
                                </article>
                                
                                
                                
                            </div>
                        </div>
                    </div>
                </template>
            </b-carousel-list> 

            <!-- mobile version -->
            <b-carousel-list class="is-hidden-tablet" v-model="test" :data="carouselItems" :items-to-show="1" :arrow="true" :arrow-hover="false">
                <template #item="list">
                    <div class="card" style="border-radius: 1rem; height: 100%;">
                        <div class="card-content">
                            <div class="content">
                                <p class="title is-6 mb-0">{{ list.location }}</p>
                                <div class="has-text-grey is-size-7">{{  getTimeFormat(list.time) }}</div>

                                <hr class="my-4" />

                                <article class="media" >
                                    <div class="media-left px-1" style="width: 48px;">
                                        <figure class="image is-48x48 has-text-centered mx-0" style="background: #fff; border: 1px solid #f5f5f5; border-radius: 100%;">
                                            <img :src="`http://openweathermap.org/img/wn/` + list.icon + `@2x.png`" alt="">
                                        </figure>
                                    </div>
                                    <div class="media-content pr-4">
                                        <div class="content is-size-7">
                                            <div>
                                                <strong class="is-size-6">{{ list.main }}</strong> 
                                                <br>
                                                <div class="has-text-grey">{{ list.description }}</div>
                                            </div>
                                            
                                        </div>
                                    </div>
                                    <div class="media-right px-1">
                                        <div class="content is-size-7">
                                            <div class="has-text-right">
                                                <strong class="is-size-6">{{ Math.round(list.temp - 273) }} °C</strong> 
                                                <br>
                                                <div class="has-text-grey">Feels like {{ Math.round(list.feelsLike - 273) }} °C</div>
                                                
                                            </div>
                                            
                                        </div>
                                    </div>
                                </article>
                                
                                
                                
                            </div>
                        </div>
                    </div>
                </template>
            </b-carousel-list> 

        </div>
       
        </div>

        <!-- <p>{{ weatherData }}</p>
        <hr>
        <p>{{ forecastData }}</p> -->

        </div>

</template>

<script>
import axios from 'axios';

export default {
    data () {
        return {
            data: ['Kuala Lumpur', 'Ipoh', 'Johor Bahru', 'Kota Bharu', 'Kota Kinabalu', 'Kuala Terengganu', 'Kuantan', 'Kuching', 'Seremban', 'Shah Alam', 
            'Ampang Jaya','Petaling Jaya', 'Port Klang', 'Sandakan', 'Alor Setar','George Town', 'Malacca', 'Putrajaya', 'Ayer Hitam', 'Batu Pahat',
            'Miri', 'Iskandar Puteri', 'Cheras', 'Subang Jaya', 'Pasir Gudang', 'Seri Kembangan', 'Kajang', 'Kota Damansara', 'Nilai', 'Nusajaya' ],
            data2: ['Kuala Lumpur', 'Klang', 'Ipoh', 'Butterworth', 'Johor Bahru', 'George Town', 'Petaling Jaya', 'Gombak', 'Hulu Langat', 'Kuala Selangor',
            'Kuantan', 'Shah Alam', 'Kota Bharu', 'Malacca', 'Kota Kinabalu', 'Seremban', 'Sandakan', 'Sungai Petani', 'Kuching', 'Miri', 
            'Kuala Terengganu', 'Langkawi', 'Putrajaya', 'Kangar', 'Labuan', 'Pasir Mas', 'Tumpat', 'Ketereh', 'Kampung Lemal', 'Pulai Chondong'],
            city: 'Kuala Lumpur',
            data3: ['Kuala Lumpur', 'Johor Bahru', 'George Town', 'Petaling Jaya', 'Gombak', 'Hulu Langat', 'Kuala Selangor',
            'Kuantan', 'Shah Alam', 'Kota Bharu', 'Malacca', 'Kota Kinabalu', 'Seremban', 'Sandakan', 'Sungai Petani', 'Kuching', 'Miri', 
            'Kuala Terengganu', 'Langkawi', 'Putrajaya', 'Kangar', 'Labuan', 'Pasir Mas', 'Tumpat', 'Ketereh', 'Kampung Lemal', 'Pulai Chondong'],
            city: 'Kuala Lumpur',
            error: '',
            lat:'',
            lon:'',
            weatherData: {},
            weatherMainData: {},
            weatherWindData: {},
            useCurrentLocation: false,
            currentLocationName: '',
            forecastData: {},
            test: 0,
            carouselItems: [],
            selectCarousel: '',
        
        }
  },
  methods:{
    info(value) {
        this.test = value
    },

    async getWeatherData(city){       
        const apiKey = '8c40c9b7a0be6aad96eb0a312e4774d2';

        axios
        .get(`https://api.openweathermap.org/data/2.5/weather?q=` + city + `&appid=` + apiKey)
        .then((response) => {

                console.log(response);
                //this.notifications = response.data;
                //this.notifications.push(response.data);
                this.weatherData = response.data;
                this.weatherMainData = response.data.main;
                this.weatherWindData = response.data.wind;
                console.log(this.weatherData);
                
            })
        .catch(function (error) {
            // handle error
            console.log(error);
        })
        .then(function () {
            // always executed
        });
    },

    async getWeatherDataByGeoCoords(lat, lon){       
        const apiKey = '8c40c9b7a0be6aad96eb0a312e4774d2';

        axios
        .get(`https://api.openweathermap.org/data/2.5/weather?lat=` + lat + `&lon=` + lon + `&appid=` + apiKey)
        .then((response) => {

                console.log(response);
                //this.notifications = response.data;
                //this.notifications.push(response.data);
                this.weatherData = response.data;
                this.weatherMainData = response.data.main;
                this.weatherWindData = response.data.wind;
                this.currentLocationName = response.data.name;
                console.log(this.weatherData);
            })
        .catch(function (error) {
            // handle error
            console.log(error);
        })
        .then(function () {
            // always executed
        });
    },

    async getForecastData(city){     
        const apiKey = '8c40c9b7a0be6aad96eb0a312e4774d2';

        axios
        .get(`https://api.openweathermap.org/data/2.5/forecast?q=` + city + `&appid=` + apiKey)
        .then((response) => {

                console.log(response);
                //this.notifications = response.data;
                //this.notifications.push(response.data);
                this.forecastData = response.data;
                console.log(this.forecastData);
            })
        .catch(function (error) {
            // handle error
            console.log(error);
        })
        .then(function () {
            // always executed
        });
    },

    async getForecastDataByGeoCoords(lat, lon){       
        const apiKey = '8c40c9b7a0be6aad96eb0a312e4774d2';

        axios
        .get(`https://api.openweathermap.org/data/2.5/forecast?lat=` + lat + `&lon=` + lon + `&appid=` + apiKey)
        .then((response) => {

                console.log(response);
                //this.notifications = response.data;
                //this.notifications.push(response.data);
                this.forecastData = response.data;
                //this.currentLocationName = response.data.name;
                console.log(this.forecastData);
            })
        .catch(function (error) {
            // handle error
            console.log(error);
        })
        .then(function () {
            // always executed
        });
    },

    carouselDefault(){
        //Default 3 weather data
        if(this.carouselItems.length === 0 && !localStorage.carouselItems){
            this.getWeatherDataForCarousel('Klang');
            this.getWeatherDataForCarousel('Ipoh');      
            this.getWeatherDataForCarousel('Butterworth');
        }

    },

    async addNewItemToCarousel(item){
        //console.log('Add item: ', item)
        await this.getWeatherDataForCarousel(item);
        const index = this.data3.indexOf(item);
        //console.log('index: ', index)
        if (index > -1) {
            this.data3.splice(index, 1);
        }
        console.log('check', this.carouselItems);

        // localStorage.carouselItems = JSON.stringify(this.carouselItems);
        // localStorage.data3 = JSON.stringify(this.data3);

    },

    async getWeatherDataForCarousel(city){       
        const apiKey = '8c40c9b7a0be6aad96eb0a312e4774d2';

        axios
        .get(`https://api.openweathermap.org/data/2.5/weather?q=` + city + `&appid=` + apiKey)
        .then((response) => {

                console.log(response);
                //this.notifications = response.data;
                //this.notifications.push(response.data);
                //var arr = [];
                var obj = {};
                obj["location"] = response.data.name;
                obj["time"] = response.data.dt;
                obj["main"] = response.data.weather[0].main;
                obj["description"] = response.data.weather[0].description;
                obj["icon"] = response.data.weather[0].icon;
                obj["temp"] = response.data.main.temp;
                obj["feelsLike"] = response.data.main.feels_like;
                //arr.push(obj);

                this.carouselItems.push(obj);
                console.log(this.carouselItems);
            })
        .catch(function (error) {
            // handle error
            console.log(error);
        })
        .then(function () {
            // always executed
        });
    },

    changeLocation() {
        this.getWeatherData(this.city);
        this.getForecastData(this.city);
    },

    inputCurrentLocation() {
        var t = this;

        navigator.permissions.query({name:'geolocation'}).then(function(result) {
            // Will return ['granted', 'prompt', 'denied']
            console.log(result.state);

            if(result.state == 'granted'){

                if(t.useCurrentLocation){   
                    t.getLocation();
                    t.getWeatherDataByGeoCoords(t.lat, t.lon);
                    t.getForecastDataByGeoCoords(t.lat, t.lon);
                }else{
                    t.getWeatherData(t.city);
                }
                
            }else{
                t.useCurrentLocation = false;
                t.getLocation();
            }
        });
        
        
    },

    preGetLocationIfGranted(){
        var t = this;

        navigator.permissions.query({name:'geolocation'}).then(function(result) {
            // Will return ['granted', 'prompt', 'denied']
            console.log('location permission', result.state);

            if(result.state == 'granted'){
                    t.getLocation();
            }
        });
    },

    getLocation() {
        if (navigator.geolocation) {
            navigator.geolocation.getCurrentPosition(this.showPosition);

        } else { 
            this.error = "Geolocation is not supported by this browser.";
        }
    },
    showPosition(position) {
        this.lat = position.coords.latitude;
		this.lon = position.coords.longitude;
    },

    getTimeFormat(unixTimestamp){
        // Create a new JavaScript Date object based on the timestamp
        // multiplied by 1000 so that the argument is in milliseconds, not seconds.
        var date = new Date(unixTimestamp * 1000);
        // Hours part from the timestamp
        var day = date.getDate();
        //var month = date.getMonth();
        const monthName = date.toLocaleString("default", {month: "short"});
        
        var hours = date.getHours();
        if(hours >= 13){
            var m = 'PM'
            hours = hours - 12;
        }else if(hours == 12){
            var m = 'PM'
        }else{
            var m = 'AM'
        }
        // Minutes part from the timestamp
        var minutes = "0" + date.getMinutes();
        // Seconds part from the timestamp
        var seconds = "0" + date.getSeconds();

        // Will display time in 10:30:23 format
        var formattedTime = day + ' ' + monthName + ', ' + hours + ':' + minutes.substr(-2) + ':' + seconds.substr(-2) + ' ' + m;

        return formattedTime;
    },

    getForecastTimeFormat(unixTimestamp){
        // Create a new JavaScript Date object based on the timestamp
        // multiplied by 1000 so that the argument is in milliseconds, not seconds.
        var date = new Date(unixTimestamp * 1000);
        // Hours part from the timestamp
        var day = date.getDate();
        //var month = date.getMonth();
        const monthName = date.toLocaleString("default", {month: "short"});
        
        var hours = date.getHours();
        if(hours >= 13){
            var m = 'PM'
            hours = hours - 12;
        }else if(hours == 12){
            var m = 'PM'
        }else{
            var m = 'AM'
        }
        // Minutes part from the timestamp
        var minutes = "0" + date.getMinutes();
        // Seconds part from the timestamp
        var seconds = "0" + date.getSeconds();

        // Will display time in 10:30:23 format
        var formattedTime = day + ' ' + monthName + ' ' + hours + ':' + minutes.substr(-2) + ' ' + m;

        return formattedTime;
    }

  },

  mounted(){
        this.getWeatherData(this.city);
        this.getForecastData(this.city);
        this.preGetLocationIfGranted();
        this.carouselDefault();

// localStorage.removeItem(carouselItems);
// localStorage.removeItem(data3);
//         console.log('LS ', localStorage.carouselItems, localStorage.data3);
//         console.log('LS JSON', JSON.parse(localStorage.carouselItems), JSON.parse(localStorage.data3));
//         var LS1 = JSON.parse(localStorage.carouselItems);
//         var LS2 = JSON.parse(localStorage.data3);
        //this.carouselItems = LS1;
        //this.data3 = LS2;

        //Clear local Storage
        //localStorage.clear();

        if(localStorage.carouselItems){ 
            
            console.log('LS JSON', JSON.parse(localStorage.carouselItems));
            this.carouselItems = JSON.parse(localStorage.carouselItems); 
        } 
        if(localStorage.data3){ 
            console.log('LS JSON', JSON.parse(localStorage.data3));
            this.data3 = JSON.parse(localStorage.data3); 
        }
  },
  watch: {
    carouselItems(newData) {
      localStorage.carouselItems = JSON.stringify(newData);
    },
    data3(newData) {
      localStorage.data3 = JSON.stringify(newData);
    }

  },

  created() {
    
  },

  updated() {
    //this.getWeatherData(this.city);
  }
}
</script>

<style>
.d-columns-fix{
    margin-left: 0px !important;
    margin-right: 0px !important;
    margin-top: 0px !important;
}

.d-shadow{
    box-shadow: 0 0.5em 1em -0.125em rgb(10 10 10 / 10%), 0 0 0 1px rgb(10 10 10 / 2%);
}

.mx-0{
    margin-left: 0px !important;
    margin-right: 0px !important;
}

.d-card{
    border-radius: 1rem;
    background: #fff;
}

html,body, #__nuxt, #__layout{
  height:100%!important;
  width: 100%!important;
}

.font-poppins{
    font-family: 'Poppins', sans-serif;
}

@media screen and (max-width: 1023px) {
    .logo{
        margin-top: 1rem;
        margin-bottom: 1rem;
    }
}

.carousel-slide {
    padding: 0.2rem;
}
.carousel-list {
    box-shadow: none !important;
}

.carousel-arrow .icon.has-icons-left {
    left: 0.75rem !important;
}
.carousel-arrow .icon.has-icons-right {
    right: 0.75rem !important;
}

::-webkit-scrollbar-thumb {
    border-radius: 3px !important;
    border: 0px solid white !important; /* should match background, can't be transparent */
    background-color: rgba(0, 0, 0, .5) !important;
}
::-webkit-scrollbar-track { 
    background-color: #fff !important; 
    border-radius: 3px !important; 
} 

</style>
