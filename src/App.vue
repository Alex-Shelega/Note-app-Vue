<template>
  <div id="noteview">
    <input type="text" placeholder="Search">
    <button v-for="note in data.notes" :key="note.id" @click="select(note.id)">
      <h2>{{ note.title }}</h2>
      <p>{{ note.note }}</p>
    </button>
  </div>
  <div id="noteedit">
    <input type="text" placeholder="Title" v-model="data.title"/>
    <textarea name="" id="" cols="30" rows="10" placeholder="Type anytyhing" v-model="data.body"></textarea>
    <div id="actions">
      <button id="save" @click="save(data.id)">Save</button>
      <button id="delete" @click="del(data.id)">Delete</button>
    </div>
  </div>
</template>

<script setup>
import { reactive } from 'vue';
import notes from './assets/notes.json' with { type: 'json' };

console.log(notes)

const data = reactive({});

data.notes = notes

function select(n){
  data.title = data.notes.find(note => note.id === n).title
  data.body = data.notes.find(note => note.id === n).note
  data.id = n
}

function save(n){
  data.notes.find(note => note.id === n).title = data.title;
  data.notes.find(note => note.id === n).note = data.body
}

function del(n){
  let index = data.notes.findIndex((val) => val.id === n);
  index !== -1 && data.notes.splice(index,1)
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
  transition: all 0.2s ease;

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

#noteview{
  height: 80vh;
  overflow: auto;
}

input:focus, textarea:focus {
  outline: none;
  border: 2px solid #4A90E2; /* Calm blue border */
  box-shadow: 0 0 5px rgba(74, 144, 226, 0.5); /* Subtle glow */
  background-color: #f9fbff; /* Slight background change to indicate focus */
}

#actions{
  display: flex;
}

#actions button{
  margin-right: 5px;
  text-align: center;
}

#save{
  background-color: green;
}

#delete{
  background-color: red;
}

#noteview input{
  all: unset;
}
</style>