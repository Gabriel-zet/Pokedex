<template>
  <div class="pokemon">
    <div class="card">
      <img src="../assets/pokeball.png" class="pokeball" />
      <div class="card-image">
        <figure class="image is-4by">
          <img :src="currentImgPrincipal" alt="Placeholder image" />
        </figure>
      </div>
      <div class="card-content">
        <div class="media">
          <div class="media-left">
            <figure class="image is-60x60">
              <img :src="currentImgAvatar" alt="Placeholder image" />
            </figure>
          </div>
          <div class="media-content">
            <br>
            <p class="title is-4">{{ num }} - {{ upperCaseName }}</p>
            <p class="subtitle is-6">Pokemon</p>
          </div>
        </div>

        <div class="content">
          Tipo: <strong>{{ pokemon.type }}</strong>
          <hr />
          Habilidades:
          <strong>{{ pokemon.hab[0] }} / {{ pokemon.hab[1] }}</strong>
          <hr />
          <button class="button is-link" @click="mudarSpriteAvatar">
            Change Avatar Image
          </button>
          _____
          <button class="button is-link" @click="mudarSpritePrincipal">
            Change Principal Image
          </button>
          <br />
          <hr />
          <time datetime="">{{ this.datagem }}</time>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      isFront: true,
      currentImgAvatar: '',
      currentImgPrincipal: '',
      datagem: 0,
      pokemon: {
        front: '',
        back: '',
        type: '',
        hab: {
          0: '',
          1: ''
        },
      },
    };
  },

  props: {
    num: Number,
    name: String,
    url: String,
  },
  computed: {
    upperCaseName: function () {
      return this.name[0].toUpperCase() + this.name.slice(1);
    },
  },
  created: function () {
    axios
      .get(this.url)
      .then((res) => {
        this.pokemon.hab[0] = res.data.abilities[0].ability.name;
        this.pokemon.hab[1] = res.data.abilities[1].ability.name;
        this.pokemon.type = res.data.types[0].type.name[0].toUpperCase() + res.data.types[0].type.name.slice(1);
        this.pokemon.front = res.data.sprites.front_default;
        this.pokemon.back = res.data.sprites.back_default;
        this.currentImgAvatar = this.pokemon.front;
        this.currentImgPrincipal = this.pokemon.back;
        var meses = [
        "Jan.", "Fev.", "Mar.", "Abr.",
        "Mai.", "Jun.", "Jul.", "Agos.",
        "Set.", "Out.", "Nov.", "Dez."
        ]
        var now = new Date;
        this.datagem = (now.getDate() + " de " + meses[now.getMonth()] + " de " + now.getFullYear()+".")
      })
      .catch((err) => {
        console.warn(err);
      });
  },
  methods: {
    mudarSpriteAvatar: function(){
      if(this.isFront){
        this.isFront = false;
        this.currentImgAvatar = this.pokemon.back;
      }else{
        this.isFront = true;
        this.currentImgAvatar = this.pokemon.front;
      }
    },
    mudarSpritePrincipal: function(){
      if(!this.isFront){
        this.isFront = true;
        this.currentImgPrincipal = this.pokemon.front;
      }else{
        this.isFront = false;
        this.currentImgPrincipal = this.pokemon.back;
      }
    }
  }
};

</script>
<style>
.pokemon {
  margin-top: 5%;
}
.pokeball {
  margin-left: 10px;
  margin-top: 20px;
  width: 20px;
}
</style>

