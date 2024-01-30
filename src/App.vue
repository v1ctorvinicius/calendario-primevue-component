<script setup lang="ts">

import { ref, onMounted } from "vue";
import 'primevue/resources/themes/lara-light-blue/theme.css'
import Calendar from 'primevue/calendar';


const eventos = [new Date(2024, 0, 10), new Date(2024, 0, 15)];
const dias = eventos.map(data => data.getDate());
console.log(dias);


let _calendario;


onMounted(() => {


  _calendario = document.querySelector('#calendario') as HTMLElement;

  const modal = document.querySelector('#modal') as HTMLElement;
  const tds = document.querySelectorAll('td');

  tds.forEach(td => {
    const span = td.querySelector('span');

    span!.addEventListener('mouseenter', (event) => {

      const { clientX, clientY } = event;

      modal!.style.left = `${clientX}px`;
      modal!.style.top = `${clientY}px`;

      modal.classList.add('show');
    });
    span!.setAttribute('style', 'cursor: pointer;');


    span!.addEventListener('mouseleave', (event) => {
      modal.classList.remove('show');
    });
  })


})

</script>

<template>
  <div>
    <Calendar id="calendario" locale="pt" dateFormat="dd/mm/yyyy" inline selectionMode="multiple" :modelValue="eventos"
      @date-select="console.log($event)" />
  </div>

  <div id="modal" class="hidden">
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
