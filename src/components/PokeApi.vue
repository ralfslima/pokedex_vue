<template>
  <div class="template">
    <!-- Quantidade de Pokémons na sua Pokedex -->
    <div class="container qtd">
      <div class="row">
        <div class="col-9"></div>
        <div class="col-3">Pokémons em sua Pokedex: {{vetor_pokemons_selecionados.length}}</div>
      </div>
    </div>

    <!-- Pesquisa -->
    <form class="container">
      <div class="row">
        <div class="col-12">
          <input type="text" placeholder="Informe o nome do Pokémon" class="form-control" v-model="termoPesquisa" />
        </div>
      </div>
    </form>
    
    <!-- Listar os Pokémons -->
    <div class="container">
      <div class="row">
        <div class="col-2" v-for="(v, indice) in vetor.filter(obj => {return obj.name.indexOf(termoPesquisa) >= 0; })" v-bind:key="indice">
          <div class="card">
            <img :src="v.sprites.front_default" >
            <button class="btn btn-primary" @click="cadastrar(v.name)">{{v.name}}</button>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: 'PokeApi',
  data(){
    return{
      // Variáveis
      vetor : [],
      termoPesquisa : '',
      vetor_pokemons_selecionados : []
    }
  },
  beforeMount(){
    // Ao carregar o componente, executa esse método
    this.obterPokemons()
  },
  methods:{

    // Método para obter todos os 493 Pokémons (Até a quarta temporada)
    obterPokemons(){
      for(let i=0; i<494; i++){
        // Você também pode fazer o fetch: fetch('https://pokeapi.co/api/v2/pokemon/'+i)
        fetch(`https://pokeapi.co/api/v2/pokemon/${i}`)
        .then(retorno => retorno.json())
        .then(pokemon => this.vetor.push(pokemon))
      }
    },

    cadastrar(nome){
      // Verifica se o Pokémon selecionado está cadastrado
      if(this.vetor_pokemons_selecionados.findIndex(n => {return n == nome}) == -1){
        this.vetor_pokemons_selecionados.push(nome);
        alert("Pokémon cadastrado!");
      }else{
        alert("Este Pokémon já está cadastrado");
      }

      
    }
  }
}
</script>


<style scoped>
  .qtd{
    margin-top: 10px;
    text-align: right;
  }

  .col-2{
    margin-bottom: 20px;
  }

  form{
    margin-top: 20px;
    margin-bottom: 40px;
  }
</style>
