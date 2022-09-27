<template>
  <div class="app pt-16 pl-16">
    <h1 class="text-2xl font-medium mb-10">Headless UI Example: Combobox</h1>
    <Form @submit="onSubmit" autocomplete="off">
      <div class="max-w-xl grid grid-cols-2 gap-8 mb-64">
        <div>
          <Field
            name="firstPerson"
            v-slot="{ field, errorMessage }"
            v-model="firstPerson"
            :rules="validateField"
          >
            <Combobox
              :multiple="true"
              :options="people"
              v-bind="field"
              key-prop="name"
              label-prop="name"
            />

            <span class="text-red-500">{{ errorMessage }}</span>
          </Field>
        </div>
        <div>
          <Field
            name="secondPerson"
            v-slot="{ field, errorMessage }"
            v-model="secondPerson"
            :rules="validateField"
          >
            <ComboboxSearch
              :multiple="true"
              :options="people"
              v-bind="field"
              key-prop="name"
              label-prop="name"
            />

            <span class="text-red-500">{{ errorMessage }}</span>
          </Field>
        </div>
      </div>
      <div class="max-w-xl grid grid-cols-2 gap-8">
        <!-- With composition API -->
        <div>
          <ComboboxComposed
            :multiple="true"
            :options="people"
            name="thirdPerson"
            v-model="thirdPerson"
            key-prop="name"
            label-prop="name"
            :rules="validateField"
          />
        </div>
        <div>
          <ComboboxSearchComposed
            :multiple="true"
            :options="people"
            name="fourthPerson"
            v-model="fourthPerson"
            key-prop="name"
            label-prop="name"
            :rules="validateField"
          />
        </div>
      </div>

      <button
        class="bg-blue-500 px-4 py-2 rounded-lg text-white mt-4 text-sm block"
        type="submit"
      >
        Submit
      </button>
    </Form>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { Field, Form } from "vee-validate";
import Combobox from "./components/Combobox.vue";
import ComboboxComposed from "./components/ComboboxComposed.vue";
import ComboboxSearch from "./components/ComboboxSearch.vue";
import ComboboxSearchComposed from "./components/ComboboxSearchComposed.vue";

const people = [
  { name: "Wade Cooper" },
  { name: "Arlene Mccoy" },
  { name: "Devon Webb" },
  { name: "Tom Cook" },
  { name: "Tanya Fox" },
  { name: "Hellen Schmidt" },
  { name: "Invalid Person" },
];
const firstPerson = ref([people[0]]);
const secondPerson = ref([people[0]]);
const thirdPerson = ref([people[1]]);
const fourthPerson = ref([people[1]]);

function validateField(value) {
  if (!value) {
    return "This field is required";
  }

  if (value.name === "Invalid Person") {
    return "This field is invalid";
  }

  return true;
}

function onSubmit(values) {
  console.log("Submitted Values: ", values);
}
</script>

<style>
html,
body,
#app {
  width: 100vw;
  height: 100vh;
}

.app {
  background: whitesmoke;
  width: 100%;
  height: 100%;
}
</style>
