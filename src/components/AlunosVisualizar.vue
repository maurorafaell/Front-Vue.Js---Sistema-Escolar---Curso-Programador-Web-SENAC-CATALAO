<template>
    <div>
      <h1>{{ aluno.nome }}</h1>
      <p><b>Endereço:</b> {{ aluno.endereco }}</p>
      <p><b>Telefone:</b> {{ aluno.telefone }}</p>
      <hr>
      <h2>Matrículas atuais</h2>
      <table class="table table-striped">
        <thead>
          <tr>
            <th>Código do Curso</th>
            <th>Nome</th>
            <th>Código matrícula</th>
            <th>Ações</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="curso in cursos" :key="curso.codigomatricula">
            <td>{{ curso.codigocurso }}</td>
            <td>{{ curso.nome }}</td>
            <td>{{ curso.codigomatricula }}</td>
            <td>                
                <button  class="btn btn-danger" @click="deleteMatricula(curso.codigomatricula)">Excluir</button>
            </td>            
          </tr>
        </tbody>
      </table>
    </div>
  </template>
 
  <script>
  import axios from 'axios';
 
  export default {
    data() {
      return {
        cursos: {},
        aluno: {}
      };
    },
    created() {
      const codigoaluno = this.$route.params.codigoaluno;
      axios.get(`apiwebsenac21.azurewebsites.net/matriculasPorAluno/${codigoaluno}`).then(response => {
        this.cursos = response.data;
      });
      axios.get(`apiwebsenac21.azurewebsites.net/alunos/${codigoaluno}`).then(response2 => {
        this.aluno = response2.data
        this.aluno = this.aluno[0]
        console.log(this.aluno)
      });
    },
    methods: {
      formatarData(data) {
        const dataObj = new Date(data);
        return `${dataObj.getDate().toString().padStart(2, '0')}/${(dataObj.getMonth()+1).toString().padStart(2, '0')}/${dataObj.getFullYear()} ${dataObj.getHours()}:${dataObj.getMinutes()}`;
 
      },
      deleteMatricula(codigomatricula) {
        axios.delete('apiwebsenac21.azurewebsites.net/matriculas/'+codigomatricula).then(() => {
            // Atualiza a lista de alunos
            this.cursos = this.cursos.filter(curso => curso.codigomatricula !== codigomatricula);
        });                    
      },
    }
  };
  </script>
