<template>
    <div>
      <form @submit.prevent="submitForm">
        <div class="form-group">          
          <label for="nome" class="form-control">Nome: </label>
          <input type="text" id="nome" v-model="aluno.nome" required class="form-control"/>      
        </div>
        <div class="form-group">
          <label for="endereco">Endereço:</label>
          <input type="text" id="endereco" v-model="aluno.endereco" required class="form-control">
        </div>
        <div class="form-group">          
          <label for="telefone" class="form-control">Telefone: </label>
          <input type="text" id="telefone" v-model="aluno.telefone" required class="form-control"/>      
        </div>
        <br>
        <button type="submit" class="btn btn-primary">Salvar</button>
      </form>
    </div>
  </template>
 
  <script>
  import axios from 'axios';
 
  export default {
    data() {
      return {
        aluno: {
          nome: '',
          endereco: '',
          telefone: '',
        }
      };
    },    
    created() {
      const codigoaluno = this.$route.params.codigoaluno;
      axios.get(`apiwebsenac21.azurewebsites.net/alunos/${codigoaluno}`).then(response => {
        this.aluno.nome = response.data[0].nome;
        this.aluno.endereco = response.data[0].endereco;
        this.aluno.telefone = response.data[0].telefone;
        console.log(this.aluno)      
      });
    },
    methods: {
      submitForm() {        
        const codigoaluno = this.$route.params.codigoaluno;        
        axios.put(`apiwebsenac21.azurewebsites.net/alunos/${codigoaluno}`, this.aluno).then(() => {
          this.$router.push({ name: 'alunos' });
        });
      }
    }
  };
  </script>
