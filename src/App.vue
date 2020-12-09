





<template>

<v-app>
  <div>
  
<v-row>
<v-app-bar
  color="green"
>

<div style="
   
    margin-left: 400px">
    <v-text-field label="Recherche" v-model="search"></v-text-field>
  
</div>
<div>
  <v-btn elevation="2" small @click="Recherche()" style="margin-left: 10px;">Recherche</v-btn>
</div>
<div style="margin-left: 400px;">
  <v-select @change="onChange($event)" :items="items" label="Recherche"></v-select>
</div>
</v-app-bar>
</v-row>
  <v-flex d-flex style=" display: flex;max-width: 600;flex-wrap: wrap;padding-top: 12px;">
   <v-layout wrap>
       <v-flex md4 v-for="item in itemListe" :key="item.recipe" style=" border-left-width: 10px; padding-left: 10px; padding-right: 10px;">
           <v-card class="card-container">
             <v-card-title >
               {{item.recipe.label}}
             </v-card-title>
            <v-img max-height="200" max-width="449" v-bind:src="item.recipe.image" @click.stop="dialog = true"  @click="show(item)"    ></v-img>
          </v-card>
       </v-flex>
   </v-layout>
</v-flex>
    <v-dialog v-model="dialog" width="600">
       <v-card>
        <v-card-title class="headline grey lighten-2" style="color: green;">
          {{this.titre}} 
             <small style="color:blue;font-size: 15px;">(Calories:{{this.calories}})</small>
            <div style="margin-top: 5px; margin-right: 200px;color:blue;font-size: 15px;">
              Liste Cautions:
            <div v-for="item in listeCautions" :key="item" >
              <small style=" margin-left: 20px;font-size: 15px;">-{{item}}</small>
            </div>
            
          </div>
       
          
        </v-card-title>


        <v-card>
    <v-card-title>
      <div style="    margin-bottom: 30px;">
                <small> Liste des Ingridient</small>

      </div>
       
      <div style="margin-right: 50px; margin-right: 300px;">
           <v-text-field
        v-model="search1"
        append-icon="mdi-magnify"
        label="Search1"
        single-line
        hide-details 
      ></v-text-field>
      </div>
      
    </v-card-title>
    <v-text-field>Liste Ingredients</v-text-field>
    <v-data-table :headers="headers" :items="desserts" :search="search1"></v-data-table>
  </v-card>
     <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            text
            @click="dialog = false"
          >
            fermer
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
</div>
</v-app>
</template>

<script>
import axios from 'axios'
export default {
  name: 'App',
   data () {
      
      return {

        headers: [
           { text: 'weight', value: 'weight' },
          { text: 'text', value: 'text' },

        ],
        desserts: [],
        listeCautions:[],
        titre:'',
        APP_ID : "393804c5",
        APP_KEY :"6cbf76e342323faf716b98b291e52fce",
        type1 :"Alcohol-free",
        type2:"gluten-free",
        search:'',
        calories:'',
        search1:'',
        selectio:'',
        dialog: false,
         
        items:[
           "Sans_alcool",
           "Sans_gluten "
      ],
      itemListe: [],
      
      }
    },

    methods : {
      //methode de recherche alaide de saisire des caracter
      Recherche() {
        const url = `https://api.edamam.com/search?q=${this.search}&app_id=${this.APP_ID}&app_key=${this.APP_KEY}`;
         axios.get(url)
           .then(response => {console.log(response.data.hits)
           this.itemListe = response.data.hits
         })

      },

      //methode pour le filtrage selon type sans_alcole ou sans_alcole
      onChange(event) {
        console.log(event)
        if(event =="Sans_alcool") {
           const url = `https://api.edamam.com/search?q=${this.search}&app_id=${this.APP_ID}&app_key=${this.APP_KEY}&Health=${this.type1}`;
         axios.get(url)
           .then(response => {console.log(response.data.hits)
          this.itemListe = response.data.hits
       })

        } 
        else {
            const url = `https://api.edamam.com/search?q=${this.search}&app_id=${this.APP_ID}&app_key=${this.APP_KEY}&Health=${this.type2}`;
         axios.get(url)
           .then(response => {console.log(response.data.hits)
          this.itemListe = response.data.hits
       })

        }
        

      },

      //methode pour louverture les detaille dune recette a l'aide d'un modale
      show(item) {
          console.log(item,"fffff")
          this.listeCautions =item.recipe.cautions
          this.titre = item.recipe.label
          this.calories = item.recipe.calories
          this.desserts=item.recipe.ingredients
      }
      

    },
    
  

  

 
};
</script>
