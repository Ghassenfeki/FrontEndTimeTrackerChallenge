<template>
  <div id="app">
    <div class="header">
      <div div class="date">
              <p > Today,{{currentDateTime()}} </p>
        </div>
      <div class="buttons">
        <button class="button1" @click="start()">{{button}}</button>
        <button class="button2" @click="onCreateTemps()">Start new</button>
      </div>
    </div>
    
     <div class="list">
      <ul>
         <li > <p class="period">Period 1</p>
        <div class="temp"><p class="time-period">{{heure()}} -{{heure()}}  </p><p class="timer">{{numero}}</p> </div></li>
        
        <li v-for="(item,i) in history" :index="i"
        :key="item.i"> <p class="period">{{nom_periods[i]}}</p>
        <div class="temp"><p class="time-period">{{heure()}} -{{heure()}}  </p><p class="timer">{{item}}</p> </div></li>
        
      </ul>
    
    </div>
     <div class="time">
      <p class="total" >Total </p>
      <a class="timer">{{total}}</a>
    </div>
   
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'TheWelcome',
  data(){
    return{
      numero: '00:00:00',
      button: 'Start',
      timer: null,
      ss: 0,
      mm: 0,
      hh: 0,
      i:1,
      history: [],
      nom_periods:[],
      total:'00:00:00',
      
    }
  },
  methods:{
    getPeriodeList(){
    axios.get("http://localhost:3000/des-times",).then(response =>{
      this.history=response;
    })
    },
    onCreateTemps(){
     
        //HERE'S SOMETHING ON TIMER
        clearInterval(this.timer);
        this.timer = null;
        this.button = 'Start';
        this.saveHistory();
        this.numero = '00:00:00';
        
        this.ss = 0;
        this.mm = 0;
        this.hh = 0;
      
    axios.post("http://localhost:3000/des-times",{
      nom_period: this.nom_period,
      
      temps_deb: this.temps_deb,
      temps_fin: this.temps_fin,
      chrono: this.date,
    })
    },
     currentDateTime() {
      const month = ["Jan","Feb","Mar","Apr","May","June","July","August","Sep","Oct","Nov","Dec"];
      const current = new Date();
      const date =current.getDate()+' '+(month[current.getMonth()]);
      const dateTime = date ;
      return dateTime;
    },heure(){
      const d = new Date();
      const hours = d.getHours() + ":" + d.getMinutes() ;
      return hours;
    },
    start(){
      
      if(this.timer !== null){
        //HERE'S SOMETHING ON TIMER
        clearInterval(this.timer);
        this.timer = null;
        this.button = 'Start';
        
      }else{
        this.timer = setInterval(() =>{
          this.runTimer();
        }, 5);
        this.button = 'Stop';
      }
    },
    clear(){
      if(this.timer !== null){
        clearInterval(this.timer);
        this.timer = null;
      }
      this.i=1;
      this.history = [];
      this.button = 'Start';
      this.numero = '00:00:00';
      this.ss = 0;
      this.mm = 0;
      this.hh = 0;
    },
    runTimer(){
      // ADD A SECOND
      this.ss++;
      // IF THE SECONDS REACH 59
      if(this.ss == 59){
        this.ss = 0; //RESET THE SECONDS
        this.mm++; //ADD A MINUTE 
      }
      // IF MINUTES REACH 59

      if(this.mm == 59){
        this.mm = 0;
        this.hh++;
      }
      // TIME FORMAT
      let format = (this.hh < 10 ? '0'+this.hh : this.hh ) + ':';
      format += (this.mm < 10 ? '0'+this.mm : this.mm ) + ':';
      format += (this.ss < 10 ? '0'+this.ss : this.ss );
      this.total= format;
      return this.numero = format;

    },
    saveHistory(){
      if(this.ss !== null){
        this.history.push(this.numero)
        this.i++;
        this.total=this.numero;
        this.nom_periods.push("Period "+this.i);
        
      }
    }
  }
}

</script>

<style>
  #app{
    display: flex;
    width: 100%;
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }



  .timer{
    color: #415C73;
    font-size: 30px;
    font-weight: 900;
    line-height: 35px;
    text-align: center;
    
  }

  .buttons{
    
    display: flex;
    

  }

  .buttons .button1{
  box-sizing: border-box;
    width: 86px;
    height: 38px;
    border: 1px solid #CC5252;
    border-radius: 5px;
    background-color: white;
    position: relative;
    text-align: center;
    font-weight: 500;
    font-size: 19px;
    line-height: 22px;
    color: #CC5252;
    text-align: center;
    margin-left: 15px;
    margin-right: 15px;
    padding: 6px;
    cursor: pointer;
    -webkit-user-select: none;
    transition: all .50s;
  }
  .buttons .button2{
    box-sizing: border-box;
    width: 126px;
    height: 38px;
    border: 1px solid #60B669;
    border-radius: 5px;
    background-color: white;
    font-style: normal;
    font-weight: 500;
    font-size: 19px;
    line-height: 22px;
    color: #60B669;
    position: relative;
    text-align: center;
    margin-left: 15px;
    margin-right: 15px;
    padding: 6px;
    cursor: pointer;
    -webkit-user-select: none;
    transition: all .50s;
  }
  .header{
    width: 80%;
    display: flex;
    flex-direction: row;
    align-items: center;
    margin-top: 120px;
    justify-content: space-between;
  }

  .buttons .button1:hover{
    background-color:#CC5252;
    color:white;
  }
  .buttons .button2:hover{
    background-color:#60B669;
    color:white;
  }
 
  ul{
    
    padding: 0px;
    width: 100%;
    
    
    
  }

  li{
    justify-content: end;
    margin-top: 4px;
    padding-top: 20px;
    padding-bottom: 10px;
    list-style: none;
    color: #fff;
    font-size: 18px;
    width: 100%;
    display: flex;
    flex-direction: row;
  justify-content: space-between;
    border-top: 4px solid #E8E8E8;
  
    
  }

  .list button{
    cursor: pointer;
    border: 0px;
    background: #fff;
    border-radius: 5px;
    padding: 8px;
    margin-bottom: 12px;
  }
  .list-div{
    height: 100%;
    min-height: 200px;
    
  }
  .list{
    height: 100%;
    width: 80%;
    
    display: flex;
    flex-direction: row;
    align-items: center;
   
    padding: 10px;
    justify-content: end;
  }
  .time{
    height: 100%;
    width: 80%;
    display: flex;
    flex-direction: row;
    align-items: center;
    padding-left: 100px;
    padding-right: 200px;
    border-bottom: 4px solid #E8E8E8;
    border-top: 4px solid #E8E8E8;
    padding: 10px;
    justify-content: end;
  }
  .date{
    font-size: 30px;
      font-weight: 900;
      line-height: 35px;
      letter-spacing: 0em;
      text-align: left;
      color: #415C73;
  }
  .total{
    font-size: 30px;
      font-weight: 900;
      line-height: 35px;
      letter-spacing: 0em;
      text-align: left;
      color: #415C73;
      margin-right: 40px;
      
  } 
  .period{
     color: #415C73;
     font-size: 20px;
    font-weight: 400;
    line-height: 55px;
    
  }
  .temp{
    display: flex;
    flex-direction: row;

  }
  .time-period{
     color: #415C73;
    font-size: 20px;
    font-weight: 400;
    line-height: 55px;
    margin-right: 30px;
  }

</style>
