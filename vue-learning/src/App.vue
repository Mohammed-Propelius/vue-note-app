<script setup>
import { ref } from 'vue';
import {v4 as uuidv4} from 'uuid';
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';

const showModal = ref(false)
// two-way binding 
const newNote = ref("")

// pushing the note array 
const notes = ref([])


// Genrating Light Colors
function generateLightColorHex() {
  let color = "#";
  for (let i = 0; i < 3; i++)
    color += ("0" + Math.floor(((1 + Math.random()) * Math.pow(16, 2)) / 2).toString(16)).slice(-2);
  return color;
}

// Adding Notes
const addNotes = () => {
  if(newNote.value.length <= 10){
    return toast.error("Need more than 10 words")
  }
  notes.value.push({
    id:uuidv4(),
    text:newNote.value,
    date:new Date(),
    backgroundColor:generateLightColorHex()
  })
  showModal.value = false;
  newNote.value = ""
}


</script>
<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea v-model="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <button @click="addNotes">Add Note</button>
        <button class="close" @click="showModal=false">Close</button>
      </div>
    </div>

    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal=true">+</button>
      </header>
      <div v-if="!notes.values.length">
        <h2>Start Your adding Your Notes by Clicking "+"</h2>
      </div>
      <div class="cards-container">
        <div v-for="note in notes" :key="note.id" class="card" :style="{backgroundColor:note.backgroundColor}">
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString("en-US") }}</p>
        </div>
      </div>
    </div>
  </main>
</template> 

<style scoped>
  main {
    height: 100vh;
    width: 100vw;
    overflow-y: hidden;
  }

  .container {
    max-width: 1000px;
    /* padding: 10px; */
    margin: 0 auto;
    overflow:hidden
  }

  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  h1 {
    font-weight: bold;
    margin-bottom: 25px;
    font-size: 75px;
  }

  header button {
    border: none;
    padding: 10px;
    width: 50px;
    height: 50px;
    cursor: pointer;
    background-color: rgb(21,20,20);
    border-radius: 100%;
    color: white;
    font-size: 20px;
  }

  .card {
    width: 225px;
    height: 225px;
    background-color: rgb(237, 182, 44);
    padding: 10px;
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-right: 20px;
    margin-bottom: 20px;
  }

  .date {
    font-size: 12.5px;
    font-weight: bold;
  }

  .cards-container {
    display: flex;
    flex-wrap: wrap;
  }

  .overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0,0,0,0.77);
    z-index: 10;
    display: flex;
    align-items: center;
    justify-content: center;
    overflow: hidden;
  }

  .modal {
    width: 750px;
    background-color: white;
    border-radius: 10px;
    padding: 30px;
    position: relative;
    display: flex;
    flex-direction: column;
  }

  .modal button {
    padding: 10px 20px;
    font-size: 20px;
    width: 100%;
    background-color: blueviolet;
    border: none;
    color: white;
    cursor: pointer;
    margin-top: 15px
  }

  .modal .close {
    background-color: rgb(193, 15, 15);
    margin-top: 7px;
  }
</style>