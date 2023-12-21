<template>
    <Navbar 
    @find="search = $event"
    :lang="lang"
    @changeLang="changeLang" 
    />
    <Notes 
    :search="search"
    :notes="filterNotes"
    @remove="removeNote"
    @changeNote="changeNote"
    :lang="lang"
    />
    <Modal
     v-show="showModal"
     @showOrClose="showModal = false"
     @addNote="addNote"
     :edit="edit"
     :editNote="editNote"
     :currentId="currentId"
     @editedNote="editedNote"
     :lang="lang"
     />
    <AddButton @click="showModal = !showModal, edit = false"/>
</template>
<script>
import AddButton from './components/AddButton.vue'
import Modal from './components/Modal.vue'
import Navbar from './components/Navbar.vue'
import Notes from './components/Notes.vue'
// import { v4 as uuidv4 } from 'uuid';
import langs from './lang'

export default {
  components: { Navbar, Notes, Modal, AddButton },
  data() {
    return {
      notes: [],
      showModal: false,
      edit: false,
      editNote: {},
      currentId: localStorage.id ? localStorage.id : localStorage.id = 0,
      lang: 'ru',
      langWords: {},
      search: ''
    }
  },
  methods: {
    addNote(obj){
      const note = {...obj}
      note.date = new Date().toLocaleString()
      note.id = ++this.currentId
      this.notes.push(note)
    },
    removeNote(id){
      const idx = this.notes.findIndex(c => c.id == id)
      this.notes.splice(idx, 1)
    },
    getNotes(){
      this.notes = JSON.parse(localStorage.notes)
    },
    changeNote(id){
      this.edit = this.showModal = true
      let pickedNote = this.notes.find(note => note.id == id)
      this.editNote = pickedNote
    },
    editedNote(noteEdited){
        this.notes.forEach(note => {
            if(note.id == noteEdited.id){
              note.title = noteEdited.title
              note.text = noteEdited.text
              note.date = noteEdited.date
            }
        })
        this.showModal = false 
    },
    changeLang(val) {
      this.lang = localStorage.lang = val
    }
  },
  watch: { // просто следит за определенным обектом
    notes: {
      handler(){
         localStorage.notes = JSON.stringify(this.notes)
      },
      deep: true
    }
  },
  computed: {// computed следит за изменением
    filterNotes(){
      if(this.search){
        let se = this.search.toLowerCase()
        const filterNotes = this.notes.filter(item => {//  Метод filter() экземпляров Array создает неполную копию часть данного массива, отфильтрованная 
          const title = item.title.toLowerCase()
          const date = item.date.toLowerCase()
          const text = item.text.toLowerCase()
          if(title.includes(se)) return item // includes это метод массива проверяет есть ли в массиве определенный элемент и возвращает true или false
          if(date.includes(se)) return item
          if(text.includes(se)) return item
        })
        return filterNotes
      }
      else return this.notes
    }
  },
  created(){// created находится те параметры которые должны выполняться до того как сайт запустился
    if(localStorage.notes){
      this.getNotes()
    }
    localStorage.lang = localStorage.lang || 'ru'
    this.lang = localStorage.lang 
    this.langWords = langs
    localStorage.words = JSON.stringify(this.langWords)
  },
  provide(){// provide чтобы отправить данные с родительского элемента в дочерние вместо props 
    return {
      words: localStorage.words ? JSON.parse(localStorage.words) : location.reload()// location.reload() перезагружает страницу
    }
  }
}
</script>
<style lang="scss">
    
</style>