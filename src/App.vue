<template>
  <div id="app">
    
    <div class="container">
      <h2>{{optionMsg}}</h2>
    </div>

    <div class="container flex">
      <div class="cmd-3">
        <label><p>Nome</p></label>
        <input type="text" v-model="nomeCadastro" placeholder="David"/>
      </div>
      
      <div class="cmd-3">
        <label><p>Sobrenome</p></label>
        <input type="text" v-model="sobrenomeCadastro" placeholder="Bowie"/>
      </div>

      <div class="cmd-3">
        <label><p>E-mail</p></label>
        <input type="text" v-model="emailCadastro" placeholder="exemplo@gmail.com"/>
      </div>
      
      <div class="cmd-3">
        <label><p>Telefone</p></label>
        <input type="text" v-model="telefoneCadastro" placeholder="(18) 12345-6789"/>
      </div>

      <div class="cmd-6">
        <label><p>Escolha o tipo de pessoa</p></label>

        <input type="radio" name="tipoPessoa" v-model="tipoPessoa" value="cpf"/>
        <label>Pessoa Física</label>
        <br/>
        <input type="radio" name="tipoPessoa" v-model="tipoPessoa" value="cnpj" />
        <label>Pessoa Jurídica</label>
        <br/>
      </div>

      <div v-if="isFisico" class="cmd-6">
        <label><p>CPF</p></label>
        <input type="text" v-model="fisicoCadastro"  placeholder="466.830.508-13"/>
      </div>

      <div v-if="isJuridico" class="cmd-6"> q
        <label><p>CNPJ</p></label>
        <input type="text" v-model="juridicoCadastro"  placeholder="55.916.580/0001-53"/>
      </div>

      <div class="cmd-12">
        <input type="submit" :value="tipoFormularioButton" v-on:click="botaoClicado()" class="botao-full">
        <p class="errorMsg">{{errorMsg}}</p>
      </div>
    </div>

    
    <TabelaDados :dados="dados" v-on:editar="editar"/>
    
  </div>
</template>

<script>
import TabelaDados from './components/TabelaDados.vue'
import cadastrosExemplo from './js/cadastrosExemplo.js'

export default {
  name: 'App',
  components: {
    TabelaDados
  },
  data: function(){
    return{
      nomeCadastro: "",
      sobrenomeCadastro: "",
      emailCadastro: "",
      telefoneCadastro: "",
      isFisico: true,
      isJuridico: false,
      juridicoCadastro : "",
      fisicoCadastro: "",
      dados: null,
      tipoPessoa: "",
      tipoFormularioButton: "Cadastrar",
      tipoFormulario: "create",
      optionMsg: "Cadastre um novo usuário",
      errorMsg: "",
      nomeAntigo: ""
    }
  },
  watch:{
    tipoPessoa: function(){
      if(this.tipoPessoa == "cpf"){
        this.isFisico = true;
        this.isJuridico = false;
      }
      else if(this.tipoPessoa == "cnpj"){
        this.isFisico = false;
        this.isJuridico = true;
      }

    }
  },
  mounted: function(){
    this.dados = cadastrosExemplo.pessoas
  },
  methods:{
    resetarEstado: function(){
      this.nomeCadastro= "",
      this.sobrenomeCadastro= "",
      this.emailCadastro= "",
      this.telefoneCadastro= "",
      this.isFisico= true,
      this.isJuridico= false,
      this.juridicoCadastro = "",
      this.fisicoCadastro= "",
      this.tipoPessoa= "",
      this.tipoFormularioButton= "Cadastrar",
      this.tipoFormulario= "create",
      this.optionMsg= "Cadastre um novo usuário",
      this.errorMsg= "",
      this.nomeAntigo= ""
    },
    botaoClicado: function(){
      if(this.tipoFormulario == "create"){
        if(this.nomeCadastro == "" || this.sobrenomeCadastro == "" || this.emailCadastro == "" || this.telefoneCadastro == "")
          this.errorMsg = "Por favor, preencha os campos faltantes";
        else{
          this.errorMsg = "";
          this.dados.pessoas.push({
            "nome": this.nomeCadastro,
            "sobrenome": this.sobrenomeCadastro,
            "email": this.emailCadastro,
            "telefone": this.telefoneCadastro,
            "pessoaJuridica": this.isJuridico,
            "cpf": this.fisicoCadastro,
            "cnpj": this.juridicoCadastro
          })
        }
      }
      else if(this.tipoFormulario == "edit"){
        var indice = 0;
        while(this.dados[indice].nome != this.nomeAntigo)
          indice++; 
        this.dados[indice].nome = this.nomeCadastro;
        this.dados[indice].sobrenome = this.sobrenomeCadastro;
        this.dados[indice].email = this.emailCadastro;
        this.dados[indice].telefone = this.telefoneCadastro;
        this.dados[indice].cnpj = this.juridicoCadastro;
        this.dados[indice].cpf = this.fisicoCadastro;
      }
      this.resetarEstado();
    },
    modificaFisico: function(){
      this.isFisico = true;
      this.isJuridico = false;
    },
    modificaJuridico: function(){
      this.isFisico = false;
      this.isJuridico = true;
    },
    editar: function(editar){
      var indice = 0;
      while(this.dados[indice].nome != editar)
        indice++; 

      this.nomeCadastro = this.dados[indice].nome;
      this.sobrenomeCadastro = this.dados[indice].sobrenome;
      this.emailCadastro = this.dados[indice].email;
      this.telefoneCadastro = this.dados[indice].telefone;
      this.juridicoCadastro = this.dados[indice].cnpj;
      this.fisicoCadastro = this.dados[indice].cpf;
      this.nomeAntigo = this.nomeCadastro;
      this.optionMsg= "Editar registro";
      this.tipoFormulario= "edit";
      this.tipoFormularioButton= "salvar";
    }
  }
  
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
