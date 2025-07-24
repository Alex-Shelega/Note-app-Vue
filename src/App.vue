<template>
  <div id="noteview">
    <button v-for="note in notes" :key="note">
      <h2>{{ note.title }}</h2>
      <p>{{ note.note }}</p>
    </button>
  </div>
  <div id="noteedit">
    <input type="text" placeholder="Title"/>
    <textarea name="" id="" cols="30" rows="10" placeholder="Type anytyhing"></textarea>
  </div>
</template>

<script setup>
import { reactive } from 'vue';
import notes from './assets/notes.json' with { type: 'json' };

console.log(notes)

const data = reactive({})
data.number = 0

function increment() {
  data.number++
  data.smth = 'smth'
}
</script>

<style scoped>
button {
  /* Instead of all: unset, let's reset only what we need */
  background-color: lightgray;
  width: 10cm;
  margin: 3px 0;
  padding: 10px;
  border-radius: 15px;
  border: none;
  cursor: pointer;

  display: flex;
  flex-direction: column;
  justify-content: center; /* vertically center content */
  height: 80px; /* enough to fit title + note nicely */
  overflow: hidden;
  text-align: left;
}

button h2, button p {
  margin: 0; /* remove default margins */
  padding: 0;
  line-height: 1.2;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap; /* prevent wrapping */
}

#noteedit{
  display: flex;
  flex-direction: column;
  height: 100%;
}

input, textarea{
  margin: 5px;
  width: 60vw;
  border: none;
}

input{
  font-size: 90px;
}

textarea{
  margin: 5px;
  width: 100%;
  flex-grow: 1; /* Makes textarea take remaining space */
  resize: none; /* Optional: allows the user to resize the height */
  font-size: 18px;
  height: 60vh;
}

#noteview, #noteedit{
  margin: 10px;
}

input:focus, textarea:focus {
  outline: none;  /* Removes the outline */
  border: none;   /* Removes border if set */
  box-shadow: none;  /* Removes any box-shadow */
}
</style>