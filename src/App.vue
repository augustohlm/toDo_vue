<script setup>
  import { reactive } from 'vue'
  import Cabecalho from './components/Cabecalho.vue'
  import Formulario from './components/Formulario.vue'
  import ListaDeTarefas from './components/ListaDeTarefas.vue'

  const estado = reactive({
    filtros: 'todas',
    tarerefaTemp: '',
    tarefas: [
      {
        titulo: 'Estudar ES6',
        finalizada: false,
      },
      {
        titulo: 'Estudar SASS',
        finalizada: false,
      },
      {
        titulo: 'Ir para a academia',
        finalizada: true,
      }
    ]
  })

  const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada)
  }

    const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada)
  }

  const getTarefasFiltradas = () => {
    const { filtros } = estado;

    switch (filtros) {
      case 'pendentes':
        return getTarefasPendentes();
        case 'finalizadas':
          return getTarefasFinalizadas();
        default:
          return estado.tarefas;
    }
  }

  const cadastraTarefa = () => {
      const tarefaNova = {
        titulo: estado.tarerefaTemp,
        finalizada: false,
      }
      estado.tarefas.push(tarefaNova);
      estado.tarerefaTemp = ''
  }

</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length" />
    <Formulario :trocar-filtro="evento => estado.filtros = evento.target.value" :tarefa-temp="estado.tarerefaTemp" :edita-tarefa-temp="evento => estado.tarerefaTemp = evento.target.value" :cadastra-tarefa="cadastraTarefa" />
    <ListaDeTarefas :tarefas="getTarefasFiltradas()"/>
  </div>
</template>

<style scoped>
  .done {
    text-decoration: line-through;
  }
</style>
