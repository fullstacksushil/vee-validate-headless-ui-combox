<template>
  <Combobox
    :multiple="multiple"
    :model-value="value || modelValue"
    @update:modelValue="updated"
  >
    <div class="relative mt-1">
      <ComboboxLabel>Search:</ComboboxLabel>
      <div
        class="relative w-full cursor-default overflow-hidden rounded-lg bg-white text-left shadow-md focus:outline-none focus-visible:ring-2 focus-visible:ring-white focus-visible:ring-opacity-75 focus-visible:ring-offset-2 focus-visible:ring-offset-iorange-300 sm:text-sm"
      >
        <ComboboxInput
          class="w-full border-none shadow-md py-2 pl-3 pr-10 text-sm leading-5 text-gray-900 focus:ring-0"
          @change="query = $event.target.value"
        />

        <ComboboxButton
          class="absolute inset-y-0 right-0 flex items-center pr-2"
        >
          <ChevronUpDownIcon class="h-5 w-5 text-gray-400" aria-hidden="true" />
        </ComboboxButton>
      </div>

      <transition
        leave-active-class="transition duration-100 ease-in"
        leave-from-class="opacity-100"
        leave-to-class="opacity-0"
      >
        <ComboboxOptions
          class="absolute w-full py-1 mt-1 overflow-auto text-base bg-white rounded-md shadow-lg max-h-60 ring-1 ring-black ring-opacity-5 focus:outline-none sm:text-sm"
        >
          <ComboboxOption
            v-slot="{ active, selected }"
            v-for="option in filteredPeople"
            :key="option[keyProp]"
            :value="option"
            as="template"
          >
            <li
              :class="[
                active ? 'text-amber-900 bg-amber-100' : 'text-gray-900',
                'cursor-default select-none relative py-2 pl-10 pr-4',
              ]"
            >
              <span
                :class="[
                  selected ? 'font-medium' : 'font-normal',
                  'block truncate',
                ]"
                >{{ option[labelProp] }}</span
              >
              <span
                v-if="selected"
                class="absolute inset-y-0 left-0 flex items-center pl-3 text-amber-600"
              >
                <CheckIcon class="w-5 h-5" aria-hidden="true" />
              </span>
            </li>
          </ComboboxOption>
        </ComboboxOptions>
      </transition>
    </div>
  </Combobox>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";
import {
  Combobox,
  ComboboxLabel,
  ComboboxInput,
  ComboboxButton,
  ComboboxOptions,
  ComboboxOption,
} from "@headlessui/vue";
import {
  CheckIcon,
  ChevronUpDownIcon,
  XCircleIcon,
} from "@heroicons/vue/20/solid";
const props = defineProps({
  options: {
    type: Array,
    required: true,
  },
  modelValue: {
    type: null,
  },
  value: {
    type: null,
  },
  keyProp: {
    type: String,
    required: true,
  },
  labelProp: {
    type: String,
    required: true,
  },
  multiple: {
    type: Boolean,
    default: false,
  },
});
const emits = defineEmits(["update:modelValue"]);
function updated(event: any) {
  console.log(event);
  // emits("update:modelValue", event);
}
const query = ref("");

const filteredPeople = computed(() =>
  query.value === ""
    ? props.options
    : props.options.filter((option: any) => {
        return option.name.toLowerCase().includes(query.value.toLowerCase());
      })
);
</script>
