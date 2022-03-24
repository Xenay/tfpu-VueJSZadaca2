<template>
<v-app>
  <v-main>
   <v-form>
    <v-container>
      <v-row>
       
          
          <v-text-field v-model="ime"
            label="Ime osobe"

          ></v-text-field>
          <v-btn
            color="accent"
            elevation="3"
            small
            text
            
            @click="getName(ime);getData()"
            >
            Generate Table
            
            
            ></v-btn>

  <v-data-table
    :headers="headers"
    :items="together"
    :items-per-page="5"
    class="elevation-1"
  ></v-data-table>

       
      </v-row>
    </v-container>
   </v-form>
  </v-main>
</v-app>
</template>

<script>
  import axios from "axios";
  import store from "@/store";

  export default {
    name: 'Home',
    data: () => ({
      
      nationality : [],
      age : [],
      gender : [],
      name: [],
      
      together: [],


      headers: [
          {
            text: 'Ime',
            align: 'start',
            sortable: false,
            value: "name",
          },
          {
            text: 'Države',
            value: "nationality",
          },
          {
            text: 'vjerojatnost države',
            value: "probab1"
          },
           {
            text: 'Godine',
            value: "age",
          },
           {
            text: 'Spol',
            value: "gender",
          },
            {
            text: 'vjerojatnost spola',
            value: "probab2"
          },
          
       ]
    }),
    
    methods: {
      getName(name) {
        this.name = name;
        store.name = name;
        console.log(store.name);
      },
      getData() {
        axios
      .get("https://api.agify.io?name=" + store.name)
      .then((response)=> {
        store.age=response.data;
      });
       axios
      .get("https://api.genderize.io?name=" + store.name)
      .then((response)=> {
        store.gender= response.data;
      });
       axios
      .get("https://api.nationalize.io?name=" + store.name)
      .then((response)=> {
        store.nationality= response.data;
        
      });
      console.log(store.nationality);
      console.log(store.gender);
      console.log(store.age);
      let arraySpojeni = [];
      this.together = [store.age,store.gender,store.nationality];
      this.together = this.together[2].country.forEach((country) =>
      //(svi array[1], svi array[2], svi array[3])
        arraySpojeni.push([this.together[0],this.together[1],country])
      );
      console.log(arraySpojeni);

      let arrayDrugi = [];

      arraySpojeni.forEach((array) =>
        arrayDrugi.push({
          
          name:array[0].name,
          age:array[0].age,
          gender:array[1].gender,
          "probab2":array[1].probability,
          nationality: array[2].country_id,
          "probab1":array[2].probability,
        })
        );
        this.together = arrayDrugi;

      ;
        
    
      }
    },
  };
</script>
