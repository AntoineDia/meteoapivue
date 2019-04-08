<template>
  <div id="search">
    <input id="input" type="search" @keyup.enter="cherchemeteo" v-model="ville">
    <div id="meteo" class="">
      <h2>{{resu}}</h2>
      <div id="resultats">
        <div class="resultats" v-for="el in infos" :key="el">
          <h3>{{jour[i]}}</h3>
          <div class='barre' :style="[{'width': el[0]/max * 100 + '%'}]">{{'⠇'+el[0] +'°C'}}</div>
          <div class='barre' :style="[{'width': el[1]/max * 100 + '%'}]">{{'⠇'+el[1] +'°C'}}</div>
          <div class='barre' :style="[{'width': el[2]/max * 100 + '%'}]">{{'⠇'+el[2] +'°C'}}</div>
          <div class='barre' :style="[{'width': el[3]/max * 100 + '%'}]">{{'⠇'+el[3] +'°C'}}</div>
          <div class='barre' :style="[{'width': el[4]/max * 100 + '%'}]">{{'⠇'+el[4] +'°C'}}</div>
          <div class='barre' :style="[{'width': el[5]/max * 100 + '%'}]">{{'⠇'+el[5] +'°C'}}</div>
          <div class='barre' :style="[{'width': el[6]/max * 100 + '%'}]">{{'⠇'+el[6] +'°C'}}</div>
          <div class='barre' :style="[{'width': el[7]/max * 100 + '%'}]">{{'⠇'+el[7] +'°C'}}</div>
          <div class='barre' :style="[{'width': el[8]/max * 100 + '%'}]">{{'⠇'+el[8] +'°C'}}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data(){
      return{
        ville : "",
        resu : "Looking for a good place?",
        infos : "",
        plusdedata : "",
        max : "",
        jour : "",
      }
    },
    methods: {
      cherchemeteo(){
        document.getElementById('input').blur();
        let url = "http://api.openweathermap.org/data/2.5/forecast?";
        url += 'APPID=15202eb8aa3527be5f895dc787c00f55';
        url += '&q=' + this.ville;
        url += '&units=metric';
        fetch(url).
          then(resp => {return resp.json()}).
          then(data => {
            this.max = 0;
            this.plusdedata = data;
            if(data.cod != 404){
              this.resu = 'City: '+ data.city.name + ' (' + data.city.country+')';
              const li = data.list; // parceque c'est long
              let mearr = []; //array de 4 éléments, météo de chaque jour
              let i = 0;
              while(i < 40){
                let mday = [];
                let mdaydone = false;
                let temp = 0;
                let wet = 0;
                let wind = 0;
                //fait une array avec chaque info pour chaque jour et la met dans l'array du dessus
                while(!mdaydone){
                  //cas spécial du premier jour qui ne commence pas à minuit
                  if(i === 0){
                    let timestart = parseInt(
                      li[0].dt_txt.charAt(11) + li[0].dt_txt.charAt(12)
                    );
                    while(timestart !== 0){
                      mday.push(0);
                      i += 3;
                      if(i === timestart){
                        mday.pop();
                        timestart = 0;
                        i = 0;
                      }
                    }
                  }
                  //préparation de la journée
                  if(this.max < li[i].main.temp){this.max = li[i].main.temp}
                  temp += li[i].main.temp;
                  wet += li[i].main.humidity;
                  wind += li[i].wind.speed;
                  mday.push(li[i].main.temp);
                  i++;
                  //fin de journée, intégration et traitement des données
                  if(mday.length % 8 === 0){
                    mdaydone = true;
                    mday.push(
                      Math.round(temp/8*100)/100,
                      Math.round(wet/8*100)/100,
                      Math.round(wind/8*100)/100,
                    );
                    mearr.push(mday);
                    //je ne veux que les données de 4 jours
                    if(mearr.length === 4){
                      i = 40;
                    }
                  }
                }
              }
            this.infos = mearr;
            } else {
              this.resu = 'What is this city?';
              this.infos = "";
            }
          });
        this.ville = "";
        this.jour = new Date;
        this.jour = ['Today','Tomorow','The day after tomorow','The day after that'];
      },
    }
  }

</script>

<style>
input {
  border-radius: 20px;
  border: none;
  padding: 10px;
  padding-left: 60px;
  font-size: 20px;
  width: 60px;
  background-image: url("https://img.icons8.com/pastel-glyph/50/000000/search.png");
  background-size: contain;
  background-repeat: no-repeat;
  background-position-x: center;
  transition: .5s;
  color:white;
  opacity: 0.4;
  }
input:hover, input:focus {
  width:  250px;
  background-position-x: left;
  color:black;
  padding-left: 43px;
  opacity: 0.9;
}
.cache{
  display: none;
}
#resultats{
  display: flex;
  flex-flow: row wrap;
  justify-content:space-evenly;
  max-width: 1200px;
  margin: auto;
}
.resultats{
  border: 2px solid #126785;
  height: auto;
  min-width: 20%;
  background-color:#5ab2ff7c;
  transition: 0.5s;
  color:white;
  margin: 5px;
  padding-left: 10px;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
}
.resultats:hover{
  background-color:#5ab2ffb9;
  box-shadow: 4px 4px 8px 8px #f0fa6aab;
  cursor: pointer;
  border-top-right-radius: 40px;
  border-bottom-right-radius: 40px;
}

h2{
  color:white;
  font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
  font-size:xx-large;
  margin-bottom: 14px;
  margin-top: 14px;
}
.barre{
  background-image: linear-gradient(to right, #051937, #562b5d, #bd3a6a, #e18345, #eba912);
  font-family:monospace;
  font-size: large;
  margin-left: -10px;
  color: #ffffff;
  text-align: left;
  padding: 7px 0px 7px 0px;
  border-radius: 0px 12px 12px 0px;
}
</style>