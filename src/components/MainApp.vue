<template>
  <v-app>
    <v-main>
      <v-container id="main-container" fluid>
        <v-row class="max-size" justify="center" dense>
          <div class="header primary">
            <v-img max-height="150" max-width="250" src="../assets/notepad_icon.png" alt="Logo Notepad" contain ></v-img>
            <div class="header-text">{{ header.text }}</div>
          </div>
        </v-row>
        <br />
        <v-row class="max-size mb-8" justify="center" dense>
          <div class="add-note">
            <v-btn block color="primary" id="btnOpenForm" @click="toggleForm()" x-large depressed>{{ this.buttons.openForm.text }}</v-btn>
            <transition name="slide-y-transition" mode="out-in">
              <v-card class="form-add-note mt-6 mb-6" flat outlined v-show="this.buttons.openForm.state">
                <v-banner rounded class="mt-5 mb-5"><h2>Add some note...</h2></v-banner>
                <v-text-field dense outlined rounded v-model="addNoteText.title" label="Title" hint="Put some title here..."></v-text-field>
                <v-textarea outlined dense rounded v-model="addNoteText.description" rows="1" label="Description" hint="Put the description of your task to remind yourself what to do..."></v-textarea>
                <div class="group-buttons">
                  <v-tooltip bottom>
                    <template v-slot:activator="{ on, attrs }">
                      <v-btn icon color="primary" @click="saveNote()" x-large v-bind="attrs" v-on="on"><v-icon x-large>mdi-check-bold</v-icon></v-btn>
                    </template>
                    <span>Save</span>
                  </v-tooltip>
                  <v-tooltip bottom>
                    <template v-slot:activator="{ on, attrs }">
                     <v-btn icon color="error" @click="toggleForm()" x-large v-bind="attrs" v-on="on"><v-icon x-large>mdi-close-thick</v-icon></v-btn>
                    </template>
                    <span>Cancel</span>
                  </v-tooltip>
                </div>
              </v-card>
            </transition>
          </div>          
        </v-row>
        <v-row class="max-size" justify="center" dense v-show="notes.length == 0">
          <v-card tile outlined plain>
            <v-card-title>
              There isn't any notes to show here...
            </v-card-title>
          </v-card>
        </v-row>
        <v-row class="max-size" justify="center" dense>    
            <v-expansion-panels tile flat popout>
                <v-expansion-panel v-for="(note, index) in notes" :key="index" class="panel-style">
                  <v-expansion-panel-header v-if="true" style="font-weight: bold; font-size: 20px;">
                    <div>
                      <v-scroll-x-transition hide-on-leave>
                        <v-icon class="mr-3" x-large v-if="note.done" :color="stateIconColor(note.done)">{{ stateIcon(note.done) }}</v-icon>
                      </v-scroll-x-transition>
                      <v-scroll-x-transition hide-on-leave>
                        <v-icon class="mr-3" x-large v-if="!note.done" :color="stateIconColor(note.done)">{{ stateIcon(note.done) }}</v-icon>
                      </v-scroll-x-transition> 
                      {{ note.title }}
                    </div>
                  </v-expansion-panel-header>
                  <v-expansion-panel-content>
                    <div class="mb-4 note-description">{{ note.description }}</div>
                    <small>Created at {{ note.dateCreated }}</small>
                    <v-divider  class="my-4">
                    </v-divider>
                    <div class="group-buttons justify-center">
                      <v-tooltip bottom>
                        <template v-slot:activator="{ on, attrs }">
                          <v-btn class="mr-5 ml-5" fab @click="changeState(index)" color="warning" v-bind="attrs" v-on="on">
                            <v-icon>mdi-pencil</v-icon>
                          </v-btn>
                        </template>
                        <span>Change State</span>
                      </v-tooltip>
                      <v-tooltip bottom>
                        <template v-slot:activator="{ on, attrs }">
                        <v-btn class="mr-5 ml-5" fab @click="deleteNote(index)" color="error" v-bind="attrs" v-on="on">
                          <v-icon>mdi-delete</v-icon>
                        </v-btn>
                        </template>
                        <span>Delete item</span>
                      </v-tooltip>
                    </div>
                  </v-expansion-panel-content>
              </v-expansion-panel>
          </v-expansion-panels>
        </v-row>
        <v-row class="max-size mt-10" justify="center" dense>
           <v-footer
              padless
              rounded
            >
              <v-card
                flat
                color="primary"
                class="indigo lighten-1 white--text text-center footer-page"
              >
                <v-card-text class="white--text py-8">
                  Este é um simples projeto pessoal de gerenciamento de tarefas desenvolvido em Vue JS + Vuetify, a fins educacionais. Desenvolvido por Klinger Matheus.
                </v-card-text>

                <v-divider></v-divider>

                <v-card-text class="white--text">
                  {{ new Date().getFullYear() }} — <strong><a class="white--text" style="text-decoration: none;" target="blank" href="https://facebook.com/klinger.matheus">Klinger Matheus</a></strong>
                </v-card-text>
              </v-card>
            </v-footer>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "MainApp",

  data: () => ({
    header: {
      text: "To-Do App",
    },
    buttons: {
      openForm: {
        text: "Add Note",
        state: false,
      }
    },
    addNoteText: {
        title: '',
        description: '',
    },
    notes: []    
  }),
  methods: {
    toggleForm() {
      return this.buttons.openForm.state = !this.buttons.openForm.state
    },
    saveNote() {
      let date = this.getDateMDY()

      this.notes.push({title: this.addNoteText.title, description: this.addNoteText.description, done: false, dateCreated: date})
      this.addNoteText.title = ''
      this.addNoteText.description = ''
    },
    getDateMDY() {
      const months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];

      let date = new Date()
      let month = date.getMonth()
      let day = date.getDate()
      let year = date.getFullYear()

      return months[month] + ' - ' + day + ' - ' + year + '  '
    },
    stateIcon (index) {
      if (index) {
        return 'mdi-check-bold'
      } else {
        return 'mdi-close-thick'
      }
    }, 
    stateIconColor(index) {
      if (index) {
        return 'success'
      } else {
        return 'error'
      }
    },
    changeState (index) {
      return this.notes[index].done = !this.notes[index].done
    },
    deleteNote (index) {
      return this.notes.splice(index, 1)
    }
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Open+Sans:wght@700&display=swap");

#main-container {
  width: 120vh;
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
}

.container {
  margin: 0;
}

.header {
  display: flex;
  padding: 25px;
  flex-wrap: wrap;
  flex-direction: row;
  align-items: center;
  justify-content: center;

  color: rgba(255,255,255,0.9);
  border-radius: 8px;
}

.header-text {
  font-family: "Open Sans";
  font-size: 48px;
  padding: 15px;
}

.max-size {
  position: relative;
  display: block;
}

.form-add-note {
  padding: 10px 50px;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active em versões anteriores a 2.1.8 */ {
  opacity: 0;
}

.group-buttons {
  display: flex;
  justify-content: space-evenly;
}

.footer-page {
  width: 100%;
}

.note-description {
  font-size: 18px;
}

.panel-style {
  border: 1px solid rgba(0,0,0,0.2);
  margin-bottom: -1px;
}
</style>