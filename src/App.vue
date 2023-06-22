<script setup>
import { reactive } from 'vue';


  const estado = reactive({
    filtro: 'todas',
    tarefas: [
      {
        titulo: 'Estudar ES6',
        finalizada: false
      },
      {
        titulo: 'Estudar SASS',
        finalizada: false
      },
      {
        titulo: 'Ir para a academia',
        finalizada: true
      },
    ]
  })

  const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada)
  }
  const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada)
  }

  const getTarefasFiltradas = () => {
    const { filtro } = estado;

    switch(filtro) {
      case 'pendentes':
        return getTarefasPendentes();
      case 'finalizadas':
        return getTarefasFinalizadas();
      default:
        return estado.tarefas;
    }
  }
</script>

<template>
    <div class="container">
      <header class="p-5 mb-4 bg-light rounded-3">
        <h1>Minhas tarefas</h1>
        <p>Você possui {{ getTarefasPendentes().length }} tarefas pendentes</p>
      </header>
      <form>
        <div class="row">
          <div class="col">
            <input type="text" placeholder="Digite aqui a descrição da tarefa" class="form-control">           
          </div>
          <div class="col-md-1">
            <button type="submit" class="btn btn-primary">Cadastrar</button>
          </div>
          <div class="col-md-2">
            <select @change="evento => estado.filtro = evento.target.value" class="form-control">
              <option value="todas">Todas as tarefas</option>
              <option value="pendentes">Tarefas Pendentes</option>
              <option value="finalizadas">Tarefas Finalizadas</option>
            </select>
          </div>
        </div>
      </form>
      {{ estado.filtro }}
      <ul class="list-group mt-4">
        <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
          <input @change="evento => tarefa.finalizada = evento.target.checked" :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox">
          <label :for="tarefa.titulo" :class="{ done: tarefa.finalizada }" class="ms-3">
            {{ tarefa.titulo }}
          </label>
        </li>
      </ul>
    </div>

</template>

<style scoped>
  .done {
    text-decoration: line-through;
  }
</style>
