<script setup lang="ts">
import { ref, onMounted, onUpdated, onBeforeUpdate, watch } from "vue";
import axios from 'axios';
import 'primevue/resources/themes/md-dark-deeppurple/theme.css'
import Calendar from 'primevue/calendar';

interface EventoInfos {
  titulo: string;
  descricao: string;
  hora_inicio: string;
  hora_fim: string;
}

interface Evento {
  data: string;
  id: string;
  eventos_dia: EventoInfos[]
}


const eventosDoMes = ref<Evento[]>([]);

axios.get('http://localhost:3000/eventos').then(resposta => {
  console.log(resposta.data)
  //@ts-ignore
  eventosDoMes.value = resposta.data.reduce((acc, valor) => {
    //@ts-ignore
    if (acc.find(e => e.data == valor.data)) {
      //@ts-ignore

      acc.find(e => e.data == valor.data).eventos_dia.push({
        titulo: valor.titulo,
        descricao: valor.descricao,
        hora_inicio: valor.hora_inicio,
        hora_fim: valor.hora_fim
      })
    }
    else {
      acc.push({
        data: valor.data,
        id: valor.id,
        eventos_dia: [
          {
            titulo: valor.titulo,
            descricao: valor.descricao,
            hora_inicio: valor.hora_inicio,
            hora_fim: valor.hora_fim
          }
        ]
      })
    }
    return acc
  }, [] as Evento[])


  console.log("?", eventosDoMes);


})


const eventoSelecionado = ref()


// const eventos = [new Date(2024, 0, 10), new Date(2024, 0, 15)];
// const eventomap: Evento[] = eventos.map((e, index) => {
//   return {
//     data: e.getDate().toString(),
//     id: index.toString(),

//     eventos_dia: [
//       {
//         titulo: e.getDate() == 10 ? 'Reuniao manhã' : "Reunião Noite",
//         descricao: 'Reuniao do setor',
//         hora_inicio: '10:00',
//         hora_fim: '11:00'
//       },
//       {
//         titulo: e.getDate() == 10 ? 'Reuniao manhã' : "Reunião Noite",
//         descricao: 'Reuniao do setor',
//         hora_inicio: '10:00',
//         hora_fim: '11:00'
//       }
//     ]

//   }
// })

const teste = ref(0);
const showModal = ref(false)
const position = ref({
  left: 0,
  top: 0
})

function execute(valor: number, event: MouseEvent) {
  console.log(valor)
  const evento = eventosDoMes.value.find(e => new Date(e.data).getDate() == valor)
  console.log(evento)
  if (evento) {
    position.value.left = event.clientX;
    position.value.top = event.clientY;
    showModal.value = true

    eventoSelecionado.value = evento
  }
}


</script>

<template>
  <div>
    <Calendar id="calendario" locale="pt" dateFormat="dd/mm/yyyy" inline selectionMode="multiple"
      :modelValue="eventosDoMes.map(e => new Date(e.data))" @date-select="console.log($event)" :pt="{}"
      @month-change="() => { teste++; console.log(teste) }">
      <template #date="slotProps">
        <div @mouseover="($event) => execute(slotProps.date.day, $event)" @mouseleave="showModal = false"
          style=" width: 100%; height: 100%; display: flex; justify-content: center; align-items: center;">
          {{ slotProps.date.day }}
        </div>

      </template>

    </Calendar>

  </div>

  <div v-if="showModal && eventoSelecionado" id="modal" :style="{ left: position.left + 'px', top: position.top + 'px' }">
    <div v-for="evento in eventoSelecionado.eventos_dia">
      <h2>{{ evento.titulo }}</h2>
      <p>Conteúdo do modal...</p>
    </div>

  </div>
</template>

<style>
td:hover {
  color: tomato;
}


#modal {
  position: absolute;
  top: 0;
  left: 0;
  z-index: 10;
  transition: visibility 0s linear 0.2s, opacity 0.2s linear;
  background-color: rgba(0, 0, 0, 0.5);
  padding: 20px;
}
</style>
