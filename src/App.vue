<script setup>
import { ref } from "vue";

const showModal = ref(false);

const notes = ref([]); // list of notes.
const input = ref(""); // listening to the input.
const error = ref("");
const getRandomColor = () => {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)"; // getting random bg color for each individual note.
};
const addNote = () => {
  if (input.value.length < 5)
    return (error.value = "Note needs to have at least 5 characters.");
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: input.value,
    date: new Date(),
    backgroundColor: getRandomColor(),
  });
  showModal.value = !showModal;
  input.value = "";
};
</script>

<template>
  <main class="h-screen w-screen">
    <div
      v-if="showModal"
      class="overlay absolute w-full h-full bg-slate-500 bg-opacity-80 z-10 flex items-center justify-center"
    >
      <div
        class="modal w-[750px] bg-slate-300 rounded-xl p-8 relative flex flex-col"
      >
        <textarea
          v-model.trim="input"
          name="note"
          id="note"
          cols="30"
          rows="10"
          class="outline-none"
        ></textarea>
        <p v-if="error" class="text-red-600 font-bold">{{ error }}</p>
        <button
          @click="addNote"
          class="rounded-xl text-2xl px-3 py-5 w-full bg-violet-500 outline-none focus:outline-none text-white cursor-pointer mt-4"
        >
          Add note
        </button>
        <button
          @click="showModal = !showModal"
          class="close mt-2 bg-teal-500 rounded-xl text-white px-3 py-5 text-2xl"
        >
          Close
        </button>
      </div>
    </div>
    <div class="container max-w-5xl p-3 m-auto bg-teal-500 rounded-3xl">
      <header class="flex justify-between items-center">
        <h1 class="font-bold mb-6 text-7xl">Post-it!</h1>
        <button
          @click="showModal = !showModal"
          class="border-none hover:scale-110 transition p-3 w-12 h-12 cursor-pointer bg-violet-500 rounded-full text-white text-3xl flex items-center justify-center"
        >
          +
        </button>
      </header>
      <div class="cards-container flex flex-wrap">
        <!-- if we don't use a key then vue will re render every item in the array when something changes. -->
        <div
          :style="{ backgroundColor: note.backgroundColor }"
          v-for="(note, index) in notes"
          :key="index"
          class="card w-56 h-56 p-3 rounded-2xl flex flex-col justify-between mr-5 mb-5"
        >
          <p class="main-text">
            {{ note.text }}
          </p>
          <p class="date text-sm font-bold">
            {{ note.date.toLocaleDateString("en-US") }}
          </p>
          <!-- <button>X</button>  style a button component -->
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped></style>
