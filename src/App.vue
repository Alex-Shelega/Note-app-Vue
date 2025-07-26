<template>
<div id="noteview">
  <div id="search-wrapper">
    <input
      id="search"
      type="text"
      placeholder="Search..."
      v-model="searchTerm"
    />
    <button id="new-note" @click="clearSelection" title="New Note">+</button>
  </div>
  <button
    v-for="note in filteredNotes"
    :key="note.id"
    @click="select(note.id)"
  >
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
import { reactive, ref, computed, watch, onMounted } from 'vue'
import defaultNotes from './assets/notes.json' with { type: 'json' }

const LOCAL_STORAGE_KEY = 'vibe-notes'

// Main reactive data
const data = reactive({
  notes: [],
  title: '',
  body: '',
  id: null
})

// Search input
const searchTerm = ref('')

// Filter notes based on search term
const filteredNotes = computed(() => {
  const term = searchTerm.value.trim().toLowerCase()
  if (!term) return data.notes
  return data.notes.filter(note =>
    note.title.toLowerCase().includes(term) ||
    note.note.toLowerCase().includes(term)
  )
})

// Load notes on mount
onMounted(() => {
  const saved = localStorage.getItem(LOCAL_STORAGE_KEY)
  if (saved) {
    try {
      data.notes = JSON.parse(saved)
    } catch (e) {
      console.warn('Could not parse saved notes. Using default.')
      data.notes = defaultNotes
    }
  } else {
    data.notes = defaultNotes
  }
})

// Auto-save notes to localStorage on change
watch(
  () => data.notes,
  (newNotes) => {
    localStorage.setItem(LOCAL_STORAGE_KEY, JSON.stringify(newNotes))
  },
  { deep: true }
)

// Select a note
function select(id) {
  const note = data.notes.find(note => note.id === id)
  if (!note) return
  data.title = note.title
  data.body = note.note
  data.id = id
}

// Save or update a note
function save(id) {
  const trimmedTitle = data.title?.trim() || ''
  const trimmedBody = data.body?.trim() || ''

  if (!trimmedTitle && !trimmedBody) {
    alert('Write some notes first!!!')
    return
  }

  const existingNote = data.notes.find(note => note.id === id)

  if (existingNote) {
    existingNote.title = trimmedTitle
    existingNote.note = trimmedBody
  } else {
    data.notes.unshift({
      id: Date.now(),
      title: trimmedTitle,
      note: trimmedBody
    })
  }

  clearSelection()
}

// Delete a note
function del(id) {
  const index = data.notes.findIndex(note => note.id === id)
  if (index !== -1) {
    data.notes.splice(index, 1)
    clearSelection()
  }
}

// Clear input fields
function clearSelection() {
  data.title = ''
  data.body = ''
  data.id = null
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
  background-color: rgb(160, 255, 160);
  border: 2px, solid, green;
  font-size: 150%;
  color: green;
}

#delete{
  background-color: rgb(255, 177, 177);
  border: 2px, solid, red;
  font-size: 150%;
  color: red;
}

#search-wrapper {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

#search {
  flex-grow: 1;
  width: auto; /* override fixed width to fill container */
  padding: 8px 14px;
  font-size: 1rem;
  border: 2px solid #bbb;
  border-radius: 25px 0 0 25px; /* rounded left corners */
  box-shadow: inset 1px 1px 4px rgba(0, 0, 0, 0.1);
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
  background: url("data:image/svg+xml,%3csvg fill='gray' height='16' viewBox='0 0 24 24' width='16' xmlns='http://www.w3.org/2000/svg'%3e%3cpath d='M15.5 14h-.79l-.28-.27a6.471 6.471 0 001.48-5.34C15.19 5.59 12.4 3 9 3S2.81 5.59 2.81 8.39c0 2.8 2.79 5.39 6.19 5.39a6.471 6.471 0 005.34-1.48l.27.28v.79l4.99 5L20.49 19l-5-5zM9 13c-2.21 0-4-1.79-4-4s1.79-4 4-4 4 1.79 4 4-1.79 4-4 4z'/%3e%3c/svg%3e") no-repeat right 10px center / 16px 16px;
}

#search:focus {
  border-color: #4A90E2;
  box-shadow: 0 0 10px rgba(74, 144, 226, 0.7);
  outline: none;
  background-color: #f9fbff;
}

/* New Note button style */
#new-note {
  width: auto !important;
  padding: 0 14px;
  background-color: #4A90E2;
  color: white;
  font-weight: bold;
  font-size: 1.5rem;
  border: 2px solid #4A90E2;
  border-left: none;
  border-radius: 0 25px 25px 0; /* rounded right corners */
  padding: 0 14px;
  height: 36px; /* match input height */
  cursor: pointer;
  transition: background-color 0.3s ease;
}

#new-note:hover {
  background-color: #357ABD;
  border-color: #357ABD;
}
</style>