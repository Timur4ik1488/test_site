<template>
  <div className="wrapper">
    <h1>Погода</h1>
    <p>Узнать погоду в {{ city == "" ? "вашем городе" : `"${city}"`}}</p>
    <input type="text" v-model="city" placeholder="Ваш город">
    <button v-if="city != '' " @click="getWeather()">Получить погоду</button>
    <button disabled v-else>Введите город</button>
    <p class="error">{{ error }}</p>

    <div v-if="info != null" style="display: flex; justify-content: center; margin-top: 50px;">
      <div style="border-top: 1px solid white; width: 70%; text-align: left; padding: 0 100px; box-sizing: border-box;">
        <div style="display: flex; justify-content: space-between;">
          <p className="elemWeather">Температура сейчас: </p>
          <p className="elemWeather">{{ showTemp }} °C</p>
        </div>
        <div style="display: flex; justify-content: space-between;">
          <p className="elemWeather">Максимальная за сегодня: </p>
          <p className="elemWeather">{{ showMaxTemp }} °C</p>
        </div>
        <div style="display: flex; justify-content: space-between;">
          <p className="elemWeather">Минимальная за сегодня: </p>
          <p className="elemWeather">{{ showMinTemp }} °C</p>
        </div>
      </div>
    </div>
  </div> 
</template>

<script>
import axios from 'axios';
export default {
  data(){
    return{
      city: '',
      error: '',
      info: null
    }
  },
  methods: {
    getWeather(){
      if(this.city.trim().length < 2) {
        this.error = "Короткое название города"
        return false;
      }
      this.error = ''

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=1828cc3f40aaa6e6a399fe408f232197`)
        .then(res => (this.info = res.data))
    }
  },
  computed: {
    showTemp(){
      return this.info.main.temp;
    },
    showMaxTemp(){
      return this.info.main.temp_max;
    },
    showMinTemp(){
      return this.info.main.temp_min;
    },
  }
}
</script>

<style scoped>
.error{
  color: rgb(194, 102, 102);
}
.wrapper{
  width: 900px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background: rgb(0, 42, 28);
  text-align: center;
  color: white;
}
.elemWeather{
  color: white;
  font-size: 14pt;
  margin: 15px 0;
}
.wrapper h1{
  margin-top: 50px;
}
.wrapper p{
  margin-top: 20px;
}
.wrapper input{
  margin-top: 20px;
  background: transparent;
  border: 0;
  border: 2px solid rgb(46, 87, 86);
  color: white;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
  transition: all 0.1s ease-in-out;
  box-shadow: inset 0 0 rgb(0, 42, 28);
}
.wrapper input:focus{
  box-shadow: inset 5px 0px rgb(46 87 86);
  padding-left: 13px;
  padding-right: 3px;
  transition: all 0.2s ease-in-out;
}
.wrapper button{
  margin-top: 20px;
  background: linear-gradient(90deg, rgba(46,87,86,1) 0%, rgba(46,87,86,1) 100%);
  border: 0;
  border-bottom: none;
  color: white;
  width: 150px;
  font-size: 14px;
  padding: 7px 10px;
  outline: none;
  border-radius: 0 5px 5px 0;
  transition: all 0.2s ease-in-out;
}
.wrapper button:hover, button:active{
  background: linear-gradient(90deg, rgba(46,87,86,1) 0%, rgba(61,122,111,1) 100%);
  transition: all 0.2s ease-in-out;
}
</style>
