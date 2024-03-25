<template>
    <div class="flex">
      <div class="w-full">
        <Listbox v-model="selectedPersons" as="div" :options="people" v-on:update:model-value="handleSelect">
          <template #label>
            <label for="diana" class="block text-sm text-left leading-5 font-medium text-gray-700">
              Materia scolastica
            </label>
          </template>
          <div class="relative">
            <span class="inline-block w-full rounded-md shadow-sm">
              <ListboxButton class="cursor-default relative w-full rounded-md border border-gray-300 bg-white pl-3 pr-10 py-2 text-left focus:outline-none focus:shadow-outline-blue focus:border-blue-300 transition ease-in-out duration-150 sm:text-sm sm:leading-5" @click="isOpen = !isOpen" :aria-expanded="isOpen.toString()" id="diana">
                <span class="block truncate">{{ selectedPersons.length < 1 ? 'Seleziona materia' : `Selezionate (${selectedPersons.length})` }}</span>
                <span class="absolute inset-y-0 right-0 flex items-center pr-2 pointer-events-none">
                  <svg class="h-5 w-5 text-gray-400" viewBox="0 0 20 20" fill="none" stroke="currentColor">
                    <path d="M7 7l3-3 3 3m0 6l-3 3-3-3" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
                  </svg>
                </span>
              </ListboxButton>
            </span>
  
     
              <ListboxOptions v-if="isOpen" class="absolute mt-1 w-full rounded-md bg-white shadow-lg" @keydown.escape="isOpen = false" ref="wrapperRef">
                <template #default="{ active, data }">
                  <div :class="{ 'text-gray-900': active, 'cursor-default select-none relative py-2 pl-8 pr-4 hover:bg-blue-50': true }" @click="handleSelect(data)" v-for="person in people" :key="person.id">
                    <span :class="{ 'font-semibold': isSelected(person), 'font-normal': !isSelected(person) }" class="block truncate">{{ person.name }}</span>
                    <span v-if="isSelected(person)" class="absolute inset-y-0 left-0 flex items-center pl-1.5">
                      <svg class="h-5 w-5" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                        <path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd"/>
                      </svg>
                    </span>
                  </div>
                </template>
              </ListboxOptions>

          </div>
        </Listbox>
        <div class="pt-1 text-sm text-left" tabindex="0" aria-label="Riepilogo materie selezionate">
          <span v-if="selectedPersons.length > 0" aria-hidden="true">Materie selezionate:</span>
          <span>{{ selectedPersons.map(person => person.name).join(', ') }}</span>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { ref, reactive, onMounted, onUnmounted } from 'vue';
  import { Listbox, ListboxButton, ListboxOptions } from "@headlessui/vue";
  
  export default {
    components: {
      Listbox,
      ListboxButton,
      ListboxOptions
    },
    setup() {
      const people = reactive([
        { id: 1, name: "Italiano" },
        { id: 2, name: "Matematica" },
        { id: 3, name: "Geografia" },
        { id: 4, name: "Musica" },
        { id: 5, name: "Inglese" },
        { id: 6, name: "Arte" },
        { id: 7, name: "Informatica" },
        { id: 8, name: "Disegno" },
        { id: 9, name: "Scienze" },
        { id: 10, name: "Biologia" }
      ]);
      const isOpen = ref(false);
      const selectedPersons = ref([]);
      const wrapperRef = ref(null);
  
      function handleSelect(value) {
        if (!selectedPersons.value.includes(value)) {
          selectedPersons.value.push(value);
        } else {
          selectedPersons.value = selectedPersons.value.filter(person => person !== value);
        }
        isOpen.value = true;
      }
  
      function isSelected(person) {
        return selectedPersons.value.includes(person);
      }
  
      function handleEscapeKey(event) {
        if (event.code === "Escape") {
          isOpen.value = false;
        } else if (event.code === "ArrowDown") {
          isOpen.value = true;
        }
      }
  
      function handleArrowDown(event) {
        if (event.code === "ArrowDown") {
          isOpen.value = true;
        }
      }
  
      onMounted(() => {
        const handleClickOutside = event => {
          if (wrapperRef.value && !wrapperRef.value.contains(event.target)) {
            isOpen.value = false;
          }
        };
  
        document.addEventListener("mousedown", handleClickOutside);
        onUnmounted(() => {
          document.removeEventListener("mousedown", handleClickOutside);
        });
      });
  
      return {
        people,
        isOpen,
        selectedPersons,
        wrapperRef,
        handleSelect,
        isSelected,
        handleEscapeKey,
        handleArrowDown
      };
    }
  };
  </script>
  