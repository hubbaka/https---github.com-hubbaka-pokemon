<template>
  <div class="container-fluid mt-5">
        <div class="row">
            <div class="col-md-4">
                <div class="card">
                    <div class="card-body">
                        MAIN MENU
                        <hr>
                    </div>
                </div>
            </div>
            <div class="col-md-8">
                <div class="card">
                    <div class="card-body">
                                <img class="pokemon-logo w-25 h-25 mb-4" src="@/assets/pokemon.png" alt="POKEMON">
                                <input v-model="search" class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
                        <hr>
                        <div class="d-flex justify-content-center">
                            <div class="row">
                                <div class="col-4 mb-4" v-for="(pokemon, i) in dataPokemon" :key="pokemon.url">
                                    <List
                                        :num ="i+1"
                                        :name="pokemon.name" 
                                        :url="pokemon.url"/>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import List from "../../components/List.vue";

import axios from 'axios'
import Modal from "@/components/Modal.vue";

export default {
    components: { List },
    data: () => {
      return {
        search: null,
        filteredPokemons: [],
        pokemons: []
      }
    },
    created: function() {
        const pokemonApi = 'https://pokeapi.co/api/v2/pokemon?limit=100000&offset=0'

        //get API from Laravel Backend
        axios.get(pokemonApi)
        .then(response => {
              
            //assign state pokemons with response data
            this.pokemons = response.data.results;
            this.filteredPokemons = response.data.results;

        }).catch(error => {
            console.log(error.response.data)
        })
    },
    computed: {
        dataPokemon() {
            if (this.search) {
                return this.filteredPokemons.filter(pokemon => {
                return this.search
                    .toLowerCase()
                    .split(" ")
                    .every(v => pokemon.name.toLowerCase().includes(v));
                });
            } else {
                return this.filteredPokemons;
            }
        }
    }
}
</script>