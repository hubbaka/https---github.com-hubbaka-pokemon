<template>
    <div class="card">
        <div class="card-body text-center">
                <img :src="currentImg" alt="Pokemon">
                <h5 class="card-title">{{ upperFirstLetter  }}</h5>
                <button type="button" class="btn btn-success" data-bs-toggle="modal" :data-bs-target="'#modalDetail'+ num">
                    Detail
                </button>
        </div>
    </div>
    <div class="modal fade" :id="'modalDetail'+ num" tabindex="-1" aria-labelledby="modalDetailLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="modalDetailLabel">{{ upperFirstLetter }}</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="d-flex justify-content-center">
                        <img :src="currentImg" alt="Pokemon">
                    </div>
                    <div>
                        <strong>Name</strong>
                        <h3>{{upperFirstLetter}}</h3>
                    </div>
                    <div>
                        <strong>Size </strong>
                            <span><strong>Height:</strong> {{pokemon.height/10}} m </span>
                            <span><strong>Weight:</strong> {{pokemon.weight/10}} kg</span>
                    </div>
                    <div>
                        <strong>Types</strong>
                        <button v-for="(value, index) in pokemon.types" :key="index">
                            {{value}}
                        </button>
                    </div>
                    <div>
                        <strong>Abilities</strong>
                        <button v-for="(value, index) in pokemon.abilities" :key="index">
                            {{value.name}}
                        </button>
                    </div>
                    <div>
                        <strong>Stats</strong>
                        <ul>
                            <li v-for="(value, index) in pokemon.stats" :key="index">{{value.stat.name}} : {{value.base_stat}}</li>
                            <!-- <li v-for="(value, index) in pokemon.base_stats" :key="index">{{value}}</li> -->
                        </ul>
                    </div>
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
            isFront: true,
            currentImg: '',
            pokemonData: {},
            pokemon: {
                front: '',
                back: '',
                height: 0,
                weight: 0,
                types:[],
                abilities:[],
                stats:[],
            },
        }
    },
    props: {
        num: Number,
        name: String,
        url: String
    },
    computed: {
        upperFirstLetter: function(){
            let newName = this.name[0].toUpperCase() + this.name.slice(1);
            return newName;
        }
    },
    created: function(){
        axios.get(this.url).then(response => {
            this.pokemonData = response.data;
            this.pokemon.height = response.data.height;
            this.pokemon.weight = response.data.weight;
            this.pokemon.front = response.data.sprites.front_default;
            this.pokemon.back = response.data.sprites.back_default;
            this.currentImg = this.pokemon.front;

            this.pokemonData.types.forEach(type => {
                this.pokemon.types.push(type.type.name);
            });

            this.pokemonData.abilities.forEach(ablt => {
                this.pokemon.abilities.push(ablt.ability);
            });

            this.pokemonData.stats.forEach(stat => {
                this.pokemon.stats.push(stat);
            });

        }).catch(err => {
            console.log(err);
        })
    },
    methods: {
        setSprite: function(){
            if(this.isFront){
                this.isFront = false;
                this.currentImg = this.pokemon.back;
            }else{
                this.isFront = true;
                this.currentImg = this.pokemon.front;
            }
        }
    },
}
</script>