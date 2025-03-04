<script setup>
import { reactive } from 'vue';

const estado = reactive({
  filtro: 'todas',
  tarefaTemp: '',
  tarefa: [
    {
      titulo: 'Estudar teste1',
      finalizada: false,  
    },
    {
      titulo: 'Estudar teste2',
      finalizada: false,
    },
    {
      titulo: 'Estudar teste3',
      finalizada: false,
    }
  ]
});

const getTarefasPendentes = () => {
  return estado.tarefa.filter(tarefa => !tarefa.finalizada)
}
const getTarefasFinalizadas = () => {
  return estado.tarefa.filter(tarefa => tarefa.finalizada)
}

const getTarefasFiltradas = () => {
  const filtro = estado.filtro

  switch (filtro) {
    case 'pendentes':
      return getTarefasPendentes();
    case 'finalizadas':
      return getTarefasFinalizadas();
    default:
      return estado.tarefa
  }
}

const cadastraTarefa = () => {
  const novaTarefa = {
    titulo: estado.tarefaTemp,
    finalizada: false
  }
  estado.tarefa.push(novaTarefa)
  estado.tarefaTemp = ''
}



</script>


<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>
        Você possui {{ getTarefasPendentes().length }} tarefas pendentes
      </p>
    </header>
    <form action="" @submit.prevent="cadastraTarefa()">
      <div class="row">
        <div class="col">
          <input :value="estado.tarefaTemp" @change="evento => estado.tarefaTemp = evento.target.value" type="text" class="form-control" placeholder="Digite aqui a descrição da tarefa">
        </div>
        <div class="col-md-1">
          <button type="submit" class="btn btn-primary">Cadastrar</button>
        </div>
        <div class="col-md-2">
          <select @change="evento => estado.filtro = evento.target.value" class="form-control">
            <option value="todas">Todas as tarefas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()" :key="tarefa.titulo">
        <input @change="evento => tarefa.finalizada = evento.target.checked" v-model="tarefa.finalizada" :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox">
        <label :class="{ done: tarefa.finalizada}" :for="tarefa.titulo" class="ms-3">{{ tarefa.titulo }}</label>
      </li>
    </ul>
  </div>
</template>

<style scoped>
  .done {
    text-decoration: line-through;
  }
</style>
