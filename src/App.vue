<script setup>
import { ref, onMounted } from 'vue'
import TodoItem from './components/TodoItem.vue'

const notes = ref([])

const newNote = ref('')

function addNote() {
  if (newNote.value.trim() === '') {
    return
  }

  notes.value.push({
    id: Date.now(),
    title: newNote.value,
    completed: false
  })

  newNote.value = ''

  saveNotes()
}

function deleteNote(id) {
  notes.value = notes.value.filter(note => note.id !== id)

  saveNotes()
}

function updateNote(updatedNote) {
  const index = notes.value.findIndex(note => note.id === updatedNote.id)

  if (index !== -1) {
    notes.value[index] = updatedNote
  }

  saveNotes()
}

function saveNotes() {
  localStorage.setItem('notes', JSON.stringify(notes.value))
}

onMounted(() => {
  const savedNotes = localStorage.getItem('notes')

  if (savedNotes) {
    notes.value = JSON.parse(savedNotes)
  }
})

</script>


<template>
  <div class="container">

    <h1>My Notes</h1>

    <div class="add-todo">
      <input
      v-model="newNote"
      type="text"
      placeholder="Enter a note"
      @keyup.enter="addNote"
      />

      <button @click="addNote">
        Add
      </button>
    </div>

    <div class="todo-list">

      <TodoItem
      v-for="note in notes"
      :key="note.id"
      :todo="note"
      @delete="deleteNote"
      @update="updateNote"
      />

    </div>

  </div>
</template>


<style>
.container {
  width: 500px;
  margin: 50px auto;
  font-family: Arial, sans-serif;
}


h1 {
  text-align: center;
}


.add-todo {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}


.add-todo input {
  flex: 1;
  padding: 10px;
}


button {
  padding: 10px 15px;
  cursor: pointer;
}


.todo-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
}
</style>
