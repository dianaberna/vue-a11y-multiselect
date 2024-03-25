<script setup>
import { ref } from "vue";
import Multiselect from "./components/Multiselect.vue";

const subjects = [
  { id: 1, name: "Italiano" },
  { id: 2, name: "Matematica" },
  { id: 3, name: "Geografia" },
  { id: 4, name: "Musica" },
  { id: 5, name: "Inglese" },
  { id: 6, name: "Arte" },
  { id: 7, name: "Informatica" },
  { id: 8, name: "Disegno" },
  { id: 9, name: "Scienze" },
  { id: 10, name: "Biologia" },
];

const selectedSubjects = ref([]);
</script>
<template>
  <div
    class="h-[100vh] w-full max-w-4xl mx-auto flex items-center justify-center"
  >
    <Multiselect
      class="w-full"
      label="Materie scolastiche"
      :options="subjects"
      option-id="id"
      option-label="name"
      placeholder="Seleziona materiale"
      summary-selected-aria-label="Riepilogo materie selezionate"
      summary-selected-label="Materie selezionate:"
      v-model:selected="selectedSubjects"
    />
  </div>
</template>
