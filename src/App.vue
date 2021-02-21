<template>
  <div class="wrapper">
    <div class="wrapper-content">

      <section>
        <div class="container">

          <message  v-if="message" :message="message"></message> 

          <newNote :note="note" @addNote="addNote"></newNote>

          <div class="note-header">
            <h1> {{ title }} </h1> 

            <search 
              :value="search" 
              placeholder="Find your note" 
              @search="search = $event"> 
            </search>
            
            <div class="icons">
              <svg :class="{ active: grid }" @click="grid = true" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" ><rect x="3" y="3" width="7" height="7"></rect><rect x="14" y="3" width="7" height="7"></rect><rect x="14" y="14" width="7" height="7"></rect><rect x="3" y="14" width="7" height="7"></rect></svg>
              <svg :class="{ active: !grid }" @click="grid = false" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="8" y1="6" x2="21" y2="6"></line><line x1="8" y1="12" x2="21" y2="12"></line><line x1="8" y1="18" x2="21" y2="18"></line><line x1="3" y1="6" x2="3" y2="6"></line><line x1="3" y1="12" x2="3" y2="12"></line><line x1="3" y1="18" x2="3" y2="18"></line></svg>
            </div>
          </div>

          <notes :notes="notesFilter" :grid="grid" @updateTitle="updateTitle" @remove="removeNote"> </notes>

        </div>
      </section>

    </div>
  </div>
</template>

<script>
  import message from '../src/components/Message' //@/components/Message.vue
  import notes from '../src/components/Notes' 
  import newNote from '../src/components/NewNote'
  import search from '../src/components/Search' 

  export default {
    components: {
      message, notes, newNote, search
    }, 
    data() {
      return {
        title: "Notes App", 
        search: '', 
        message: null,
        grid: true,
        note: {
            title: "",
            priority: "Standart",
            description: "",
            contenteditable: false,
        },
        notes: [

        ],       
      }
    },
    mounted() {
      if(localStorage.getItem('listNotes')) {
        try {
          this.notes = JSON.parse(localStorage.getItem('listNotes'));
        } catch(e) {
          localStorage.removeItem('listNotes');
        }
      }
    },
    computed: {
      notesFilter() {
        let array = this.notes,
            search = this.search

        if(!search) return array;

        search = search.trim().toLowerCase();

        array = array.filter(function(item) {
          item = item.title.toLowerCase();

          if(item.indexOf(search) != -1) {
            return item
          }
        })

        return array;
      },
    },
    methods: {
        addNote() {
            let {title, description, priority, contenteditable} = this.note;

            if(title === '') {
                this.message = "title can't be blank!";
                return false;
            }
            this.notes.push({
                title,
                description,
                priority,
                contenteditable,
                date: new Date(Date.now()).toLocaleString()
            });
            localStorage.setItem('listNotes', JSON.stringify(this.notes));
            this.note.title="";
            this.note.description="";
            this.message = null;
        },
        removeNote(index) {
          this.notes.splice(index, 1);
          localStorage.setItem('listNotes', JSON.stringify(this.notes));
        },
        updateTitle(data) {
          this.notes[data.index].contenteditable = false;
          this.notes[data.index].title = data.title;
          this.notes[data.index].date = new Date(Date.now()).toLocaleString();
          localStorage.setItem('listNotes', JSON.stringify(this.notes));
        }
    }

  }

</script>

<style lang="scss">
  .note-header {    
    margin-top: 32px;
  }
</style>
