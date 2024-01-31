<script setup lang="ts">

import { ref, onMounted, onUpdated, onBeforeUpdate, watch } from "vue";
import 'primevue/resources/themes/md-dark-deeppurple/theme.css'
import Calendar from 'primevue/calendar';


const eventos = [new Date(2024, 0, 10), new Date(2024, 0, 15)];
const dias = eventos.map(data => data.getDate());
const meses = eventos.map(data => data.getMonth());

let _tds;

const teste = ref(0);

const showModal = ref(false)

const position = ref({
  left: 0,
  top: 0
})

onBeforeUpdate(() => {
  console.log("Antes de atualizar");
})

onMounted(() => {
  // setup();
})

// function setup() {
//   const modal = document.querySelector('#modal') as HTMLElement;
//   _tds = document.querySelectorAll('td');

//   _tds.forEach(td => {
//     const span = td.querySelector('span');

//     span!.addEventListener('mouseenter', (event) => {
//       const targetElement = event.target! as HTMLElement;
//       console.log("Elemento: ", targetElement.innerHTML);

//       if (dias.includes(Number.parseInt(targetElement.innerHTML))) {
//         console.log("É AQUI");

//         const { clientX, clientY } = event;

//         modal!.style.left = `${clientX}px`;
//         modal!.style.top = `${clientY}px`;

//         modal.classList.add('show');
//       }
//     });

//     span!.addEventListener('mouseleave', (event) => {
//       modal.classList.remove('show');
//     });

//     span!.setAttribute('style', 'cursor: pointer;');
//   })





function execute(valor: number, event: MouseEvent) {
  if (dias.includes(valor)) {
    position.value.left = event.clientX;
    position.value.top = event.clientY;
    showModal.value = true
  }
}
</script>

<template>
  <div>
    <Calendar id="calendario" locale="pt" dateFormat="dd/mm/yyyy" inline selectionMode="multiple" :modelValue="eventos"
      @date-select="console.log($event)" :pt="{}" @month-change="() => { teste++; console.log(teste) }">

      <template #date="slotProps">

        <div @mouseover="($event) => execute(slotProps.date.day, $event)" @mouseleave="showModal = false"
          style=" width: 100%; height: 100%; display: flex; justify-content: center; align-items: center;">
          {{ slotProps.date.day }}
        </div>

      </template>

    </Calendar>

  </div>

  <div id="modal" :class="{ show: showModal }" :style="{ left: position.left + 'px', top: position.top + 'px' }">
    <h2>Título do Modal</h2>
    <p>Conteúdo do modal...</p>
    <button id="fechar-modal">Fechar</button>
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
