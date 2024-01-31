<script setup lang="ts">

import { ref } from "vue";
import 'primevue/resources/themes/md-dark-deeppurple/theme.css'
import Calendar from 'primevue/calendar';

const apiResponse = ref<Evento[]>([
  {
    dia: 10,
    mes: 0,
    ano: 2024,
    titulo: 'Reunião manhã',
    descricao: 'Reunião do grupo'
  },
  {
    dia: 10,
    mes: 0,
    ano: 2024,
    titulo: 'Reunião tarde',
    descricao: 'Reunião do grupo'
  },
  {
    dia: 15,
    mes: 0,
    ano: 2024,
    titulo: 'Entrega',
    descricao: 'Entrega de projeto'

  }
]);

interface Evento {
  dia: number;
  mes: number;
  ano: number;
  titulo: string;
  descricao: string;
}

const eventos = apiResponse.value.map(evento => new Date(evento.ano, evento.mes, evento.dia));

const dias = apiResponse.value.map(data => data.dia);

const meses = apiResponse.value.map(data => data.mes);
const anos = apiResponse.value.map(data => data.ano);
const titulos = apiResponse.value.map(data => data.titulo);
const descricoes = apiResponse.value.map(data => data.descricao);

const showModal = ref(false);
const tituloModal = ref('');
const descricaoModal = ref('');

const posicaoModal = ref({
  left: 0,
  top: 0
})

let _eventosDoDia: Evento[];

function execute(valor: number, event: MouseEvent) {
  // const eventoDoDia = apiResponse.value.find(event => event.dia === valor);
  const _eventosDoDia = apiResponse.value.filter(event => event.dia === valor);
  console.log("eventos: ", _eventosDoDia);

  const titulosModal = _eventosDoDia.filter(event => event.titulo);
  const descricoesModal = _eventosDoDia.filter(event => event.descricao);

  const tituloDoEvento = _eventosDoDia ? _eventosDoDia.titulo : '';
  const descricaoDoEvento = _eventosDoDia ? _eventosDoDia.descricao : '';


  if (dias.includes(valor)) {
    posicaoModal.value.left = event.clientX + 20;
    posicaoModal.value.top = event.clientY + 20;
    showModal.value = true
    tituloModal.value = tituloDoEvento;
    descricaoModal.value = descricaoDoEvento;
  }
}
</script>

<template>
  <div>

    <Calendar id="calendario" locale="pt" inline selectionMode="multiple" :modelValue="eventos"
      @date-select="console.log($event)">

      <template #date="slotProps">

        <div @mouseover="($event) => execute(slotProps.date.day, $event)" @mouseleave="showModal = false"
          style=" width: 100%; height: 100%; display: flex; justify-content: center; align-items: center;">
          {{ slotProps.date.day }}
        </div>

      </template>

    </Calendar>

  </div>

  <div id="modal" :class="{ show: showModal }" :style="{ left: posicaoModal.left + 'px', top: posicaoModal.top + 'px' }">
    <div v-for="evento in _eventosDoDia">
      {{ _eventosDoDia }}
      <h2>{{ evento.titulo }}</h2>
      <p>{{ descricaoModal }}</p>

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
  visibility: hidden;
  opacity: 0;
  transition: visibility 0s linear 0.2s, opacity 0.2s linear;
  background-color: rgba(0, 0, 0, 0.5);
  padding: 20px;
}

#modal.show {
  visibility: visible;
  opacity: 1;
  transition: visibility 0s linear 0.2s, opacity 0.2s linear;


}
</style>
