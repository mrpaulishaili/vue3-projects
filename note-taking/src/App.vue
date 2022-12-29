<script setup>
import { ref } from 'vue';

const showModal = ref(false);
const newNote = ref('');
const errorMessage = ref('');
const notes = ref([]);

const addNote = () => {
  if (newNote.value.length < 10) {
    return (errorMessage.value = 'Note need to be at least 10 characters long');
  }

  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date(),
    bgColor: getRandomColor(),
  });
  showModal.value = false;
  newNote.value = '';
  errorMessage.value = '';
};

function getRandomColor() {
  let color = 'hsl(' + Math.round(Math.random() * 360) + ', 100%, 75%)';

  return color;
}
</script>

<template>
  <main>
    <div class="overlay" v-if="showModal">
      <div class="modal">
        <textarea
          name="note"
          id="note"
          cols="30"
          rows="10"
          v-model.trim="newNote"
        ></textarea>

        <p class="error" v-if="errorMessage">{{ errorMessage }}</p>

        <button @click="addNote">Add note</button>
        <button class="close" @click="showModal = false">Close</button>
      </div>
    </div>

    <div class="container">
      <header>
        <h1>Notes</h1>

        <button @click="showModal = true">+</button>
      </header>

      <div class="cards-container">
        <div
          class="card"
          v-for="(note, id) in notes"
          :key="id"
          :style="{ backgroundColor: note.bgColor }"
        >
          <p class="main-text">
            {{ note.text }}
          </p>
          <p class="date">{{ note.date.toLocaleDateString('en-US') }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  background-color: aliceblue;
  height: 100vh;
  width: 100vw;
}

.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
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
  background-color: rgba(21, 20, 20);
  border-radius: 100%;
  color: white;
  font-size: 20px;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.card {
  width: 225px;
  height: 225px;
  background-color: rgb(237, 182, 44);
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 10px;
  margin-right: 10px;
  margin-left: 10px;
}

.date {
  font-size: 12.5px;
  font-weight: bold;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;

  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  width: 750px;
  background-color: #fff;
  border-radius: 10px;
  padding: 30px;
  display: flex;
  flex-direction: column;
}

.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: blueviolet;
  border: none;
  cursor: pointer;
  color: #fff;
  margin-top: 15px;
}

button.close {
  background-color: red;
  margin-top: 5px;
}
.error {
  margin-top: 5px;
  font-weight: bold;
  color: red;
}
</style>
