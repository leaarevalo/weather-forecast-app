<template>
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <title>Document</title>
  </head>
  <body>
    <div id="app">
   <div>
  <b-jumbotron header="Weather forecast app" lead="Para buscar nuevas ciudades, por favor presione el boton 'Reset'">
  </b-jumbotron>
</div>
    <!-- Jumbotron arriba -->
    <div class="forms">
    <b-form @submit.prevent="onSubmit" @reset="onReset" >
      <b-form-group
        id="input-group-1"
        label="Diferencias clima"
        label-for="input-1"
      >
      <div class="inputClass">
      
       <b-form-input
          id="input-1"
          v-model="origen"
          required
          placeholder="Ciudad origen"
        ></b-form-input>
        </div>
        <div class="inputClass">
        <b-form-input
          id="input-2"
          v-model="destino"
          required
          placeholder="Ciudad destino"
        ></b-form-input>
        </div>
      </b-form-group>
      <div class="buttons">
        <div><b-button type="submit" variant="primary">Buscar</b-button></div>
        <div><b-button type="reset" variant="danger">Reset</b-button></div>
      
      
      </div>
    </b-form>
    </div>
    <div class="tables">
      <b-container class="bv-example-row"  >
  <b-row >
    <b-col v-if=" typeof climas[0] !=='undefined'">
      <h3 class="titleCity">{{climas[0].data.city.name}}</h3>
      <div v-for="cl in climas[0].data.list" v-bind:key="cl.id">
        <div class="porDia" v-if="cl.dt_txt">
          <div class="icon"><b-icon v-if ="cl.main.temp_max > 27" icon="brightness-high" variant="warning" font-scale="2.5"></b-icon>
          <b-icon v-else icon="cloud-fill" variant="primary" font-scale="2.5"></b-icon>
          </div>
          <p>Dia: {{cl.dt_txt.split(" ")[0]}} Hora {{cl.dt_txt.split(" ")[1]}}</p>
          <span>
       Min: {{cl.main.temp_min}}°
       Max:{{cl.main.temp_max}}°
        </span>
        </div>
      </div>
    </b-col>
    <b-col v-if=" typeof climas[1] !=='undefined'">
      <h3 class="titleCity">{{climas[1].data.city.name}}</h3>
      <div v-for="cl in climas[1].data.list" v-bind:key="cl.id">
        <div class="porDia">
          <div class="icon"><b-icon v-if ="cl.main.temp_max > 27" icon="brightness-high" variant="warning" font-scale="2.5"></b-icon>
          <b-icon v-else icon="cloud-fill" variant="primary" font-scale="2.5"></b-icon>
          </div>
          
          <p>Dia: {{cl.dt_txt.split(" ")[0]}} Hora {{cl.dt_txt.split(" ")[1]}}</p>
          <span>
        Min:  {{cl.main.temp_min}}°
        Max: {{cl.main.temp_max}}°
        </span>
        </div>
      </div>
      </b-col>
  </b-row>
  </b-container>
    </div>
    <!-- FORMULARIO ARRIBA -->
  
  </div> 
  </body>
  </html>
</template>

<script>
import axios from 'axios';
export default {
  name: 'App',
  data (){
    return {
       api_key: "d97b14127ba8bb3b6e5761f1c3453b5e",
      origen: "",
      destino: "",
      climas: [],
      }
    
    
  },
  methods: {
    onSubmit() {
        this.getWeather(this.origen).then(res => this.setResult(res)).then(()=>{
          this.getWeather(this.destino).then(res=>this.setResult(res))
        });
        
      },
      onReset(){
        this.origen = "";
        this.destino = "";
        this.climas.splice(0,2);
        
      },
    getWeather(city){
      
     return axios.get(`https://api.openweathermap.org/data/2.5/forecast?q=${city}&appid=${this.api_key}&units=metric`)
        //.then(res => this.setResult(res))
        
    },
    setResult(res){
      this.climas.push(res);
      
    }
  }

}
</script>

<style>
/* #app {
  
} */

.forms{
  display: flex;
  flex-direction: column;
  width: auto;
  justify-content: center;
  align-items: center;
}
.buttons{
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
}
.inputClass{
  width: 400px;
  margin-bottom: 15px;
}
.titleCity{
  text-align: center;
}
.porDia{
  display: flex;
  flex-direction: column;
  margin-top: 20px;
  height: 100px;
  height: 150px;
  align-items: center;
  justify-content: center;
  border: 1px solid rgba(0, 0, 0, 0.3);
  background-color: whitesmoke;
  box-shadow: 0px 0px 16px rgba(0,0, 0, 0.25);
  border-radius: 16px 16px 16px 16px;
}
.icon{
  align-self: flex-end;
  padding-right: 10px;
}
.container{
    display: block;
    height: 80px;
}
span{
  font-weight: bold;
  font-style: italic;
}
p{
  font-style: oblique;
}
.temp{
  display: flex;
  justify-content: space-around;
  border: 1px solid rgba(0, 0, 0, 0.3);
}
</style>
