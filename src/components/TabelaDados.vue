<template>
    <div class="container">
        <table>
            <tr>
                <th>Nome</th>
                <th>E-mail</th>
                <th>Opções</th>
            </tr>
            <tr v-for="cadastro in dados" v-bind:key="cadastro.nome" >
                <td>{{concatenaNomes(cadastro)}}</td>
                <td>{{cadastro.email}}</td>
                <td>
                    <span class="opcao fa fa-pencil" v-on:click="editar(cadastro.nome)"></span>
                    <span class="opcao fa fa-times" v-on:click="remover(cadastro.nome)"></span>
                </td>
            </tr>
        </table>
    </div>
</template>

<script>
export default {
    name: 'TabelaDeDados',
    props:[
        'dados'
    ],
    data: function(){
        return{
            cadastros: this.dados
        }
    },
    methods: {
        concatenaNomes: function(cadastro){
            return cadastro.nome + " " + cadastro.sobrenome
        },
        editar: function(nome){
            this.$emit('editar', nome);
        },
        remover: function(nome){
            if(confirm('Você tem certeza que quer apagar este registro?')){
                var remove = nome;
                var indice = 0;

                while( this.dados[indice].nome != remove )
                    indice++;
                this.dados.splice(indice, 1);
            }
        }
    }
}
</script>