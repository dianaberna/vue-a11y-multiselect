<script setup>
import {
  Listbox,
  ListboxLabel,
  ListboxButton,
  ListboxOptions,
  ListboxOption,
} from "@headlessui/vue";

defineProps({
  label: {
    type: String,
    required: false,
  },
  options: {
    type: Array,
    default: [],
  },
  optionKey: {
    type: String,
    default: "id",
  },
  optionLabel: {
    type: String,
    default: "id",
  },
  placeholder: {
    type: String,
    required: true,
  },
  selected: {
    type: Array,
    default: [],
  },
  summarySelectedAriaLabel: {
    type: String,
    required: true,
  },
  summarySelectedLabel: {
    type: String,
    required: false,
  },
});

defineEmits(["update:selected", "update:open"]);
</script>
<template>
  <div class="flex">
    <div class="w-full">
      <Listbox
        as="div"
        :modelValue="selected"
        @update:modelValue="(value) => $emit('update:selected', value)"
        multiple
      >
        <ListboxLabel
          v-if="label"
          for="listbox-id"
          class="block text-sm text-left leading-5 font-medium text-gray-700"
        >
          {{ label }}
        </ListboxLabel>

        <div class="relative">
          <span class="inline-block w-full rounded-md shadow-sm">
            <ListboxButton
              id="listbox-id"
              class="cursor-default relative w-full rounded-md border border-gray-300 bg-white pl-3 pr-10 py-2 text-left focus:outline-none focus:shadow-outline-blue focus:border-blue-300 transition ease-in-out duration-150 sm:text-sm sm:leading-5"
            >
              <span class="block truncate">{{
                selected.length < 1
                  ? placeholder
                  : `Selezionate (${selected.length})`
              }}</span>
              <span
                class="absolute inset-y-0 right-0 flex items-center pr-2 pointer-events-none"
              >
                <svg
                  class="h-5 w-5 text-gray-400"
                  viewBox="0 0 20 20"
                  fill="none"
                  stroke="currentColor"
                >
                  <path
                    d="M7 7l3-3 3 3m0 6l-3 3-3-3"
                    stroke-width="1.5"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  />
                </svg>
              </span>
            </ListboxButton>
          </span>
        </div>

        <ListboxOptions
          class="absolute mt-1 w-full max-w-4xl rounded-md bg-white shadow-lg"
        >
          <ListboxOption
            v-slot="{ active, selected }"
            v-for="option in options"
            :key="option[optionKey]"
            :value="option"
          >
            <div
              class="relative"
              :class="{
                'text-gray-900': active,
                'cursor-default select-none relative py-2 pl-8 pr-4 hover:bg-blue-50': true,
              }"
            >
              <span
                :class="{
                  'font-semibold': selected,
                  'font-normal': !selected,
                }"
                class="block truncate"
                >{{ option[optionLabel] }}</span
              >
              <span
                v-if="selected"
                class="absolute inset-y-0 left-0 flex items-center pl-1.5"
              >
                <svg
                  class="h-5 w-5"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 20 20"
                  fill="currentColor"
                >
                  <path
                    fill-rule="evenodd"
                    d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"
                    clip-rule="evenodd"
                  />
                </svg>
              </span>
            </div>
          </ListboxOption>
        </ListboxOptions>
      </Listbox>
      <div
        v-if="selected.length"
        class="pt-1 text-sm text-left"
        tabindex="0"
        :aria-label="summarySelectedAriaLabel"
      >
        <span v-if="summarySelectedLabel" class="mr-1" aria-hidden="true">
          {{ summarySelectedLabel }}
        </span>
        <span>{{ selected.map((option) => option.name).join(", ") }}</span>
      </div>
    </div>
  </div>
</template>
