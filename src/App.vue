<script setup>
import {ref} from "vue";
const showModal = ref(false) // koga ke klikneme na New Note sakame ovaa sostojba da se promene vo true a toa se prave so dodavanje na eventListener
const newNote = ref("")
const errorMessage = ref("")
const notes = ref([])

//ovaa funkcija ne e vue tuku pure JS
function getRandomColor() {
  return "hsl(" + Math.random() * 256 + ", 20%, 75%)";
}

const addNote = () => {
  if(newNote.value.length <= 10) {
    return errorMessage.value = "Note needs to be 10 characters or more!"  
  } 
  if(newNote.value.length >= 160) {
    return errorMessage.value = "Note can be between 10 and 140 characters!"
  }
  notes.value.push({
    //id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor(),
  })
  showModal.value = false;
  errorMessage.value = ""; // sama si ga dodade ovoa funkciice: da ne stoe zacuvana error message
  newNote.value = "";// note ednas ke se napise i ke se dodade so add buton 
  //posle koga user saka da napise druga note ke mu se pojave prazna textarea namesto so tekstot sto bil prethodno napisan
}
</script>

<template>
  <main>
  <div v-show="showModal" class="overlay"> <!-- sakame da se pojave koga showModal e vo true sostojba 
    (koga sakame da se pojave textarea z apisuvanje note) a toa se prave so v-if ili so v-show, 
    koi e se vikat direktivi ili directives; v-show e podobro da se koriste poradi SEO i moze da gu promovira stranata-->
    <div class="modal" id="#app">
      <!--{{ newNote }}-->
      <textarea v-model="newNote" name="note" id="note" cols="30" rows="10"></textarea><!-- so v-model="newNote"
      se povrzuvat const newNote vo koja ke se zacuvuva note sto ke ja pisuva user i textarea t.e. mestoto kade
      se pisuva note -->
      <P v-if="errorMessage" > {{ errorMessage }} </P>
      <div class="add-close-buttons">
        <button @click="addNote" >Add note</button>
        <button @click="showModal = false" class="close">Close</button>
      </div>
    </div>
  </div>
  
    <div class="conteiner">
      <header>
        <!--{{ notes }}-->
        <h1>Notes</h1>
        <!-- <h1>Notes {{ showModal }}</h1> so otvaranje na stranata stoe false koga se klikne na + se smene vo 
        true. Se koriste za testiranje. -->
        <button @click="showModal = true">New Note</button> <!-- nacin na stavanje eventListener  -->
      </header>

      <div class="card-conteiner">
        <div v-for="note in notes" 
          :key="note.id"
          class="card" 
          :style="{ backgroundColor: note.backgroundColor}"
        >
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString("de") }}</p>
        </div>
      </div>

    </div>
  </main>
</template>

<style scoped>

main {
  height: 100vh;
  width: 100vw;
}
.conteiner {
  max-width: 1000px;
  margin: 0 auto;
  padding: 10px;
}
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
h1 {
  font-size: 75px;
  font-weight: bold;
  margin-bottom: 25px;
}
header button {
  border: none;
  width: 200px;
  height: auto;
  padding: 10px;
  cursor: pointer;
  background-color: #2C3D50;
  color: whitesmoke;
  font-size: 20px;
  border-radius: 5px;
}
.card {
  width: 225px;
  height: 225px;
  padding: 10px;
  border-radius: 5px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-left: 20px;
  margin-bottom: 20px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.2);
}
.date {
  font-size: 12px;
  font-weight: bold;
}
.card-conteiner {
  display: flex;
  flex-wrap: wrap; /*rede gi horizontalno */
  gap: 20px;
}
.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgb(0, 0, 0, 0.47);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}
.modal {
  width: 750px;
  background-color: whitesmoke;
  border-radius: 15px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}
textarea {
  border-radius: 5px;
  font-family: Inter, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Fira Sans', 'Droid Sans', 'Helvetica Neue', sans-serif;
}
.add-close-buttons{
  display: flex;
}
.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 50%;
  background-color: #2C3D50;
  color: whitesmoke;
  cursor: pointer;
  border: none;
  border-radius: 5px 0 0 5px;
  margin-top: 15px;
}
.modal .close {
  background-color: #FC2947;
  border-radius:0 5px 5px 0;
}
.modal p {
  color: #FC2947;
}
@media(max-width: 768px) {
  header {
    display: grid;
    justify-content: center;
    align-items: center;
  }

  h1 {
    margin-bottom: 15px;
  }
  .card-conteiner {
    display: grid;
    justify-content: center;
    margin: 50px 0;
  }
  .card {
    margin: 0;
  }
  .modal {
    margin: 0 10px;
  }
}
</style>