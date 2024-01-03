<template>
  <q-page class="flex column flex-center justify-center q-px-lg">

    <div class="about q-pa-lg q-mt-lg">

      <h4>Soul Fragments is a digital sanctuary where your secrets find a voice in the vast expanse of the internet. Unburden your soul and unveil the depths of your untold stories here.</h4>

    </div>

    <div class="fragment q-pa-lg q-my-lg">

      <p class="text-h4 q-mx-auto">Send a little fragment of your soul to the world.</p>

      <q-form @submit="addFragment" class="q-gutter-md">

        <q-input class="fragment-text text-h5" v-model="fragment" filled :rows="20" type="textarea" placeholder="Your soul fragment"/>

        <q-input class="text-h5" v-model="pseudonym" filled placeholder="Your pseudonym"/>

        <q-btn class="q-mb-" label="Submit" type="submit" color="primary"/>

        <q-dialog v-model="successDialog">
          <q-card>
            <q-card-section class="q-pt-md q-pb-md">
              <q-card-title class="text-h5">Thank you sharing a soul fragment!</q-card-title>
            </q-card-section>
  
            <q-card-actions align="right">
              <q-btn label="Close" color="primary" @click="closeDialog" />
            </q-card-actions>
          </q-card>
        </q-dialog>

        <q-dialog v-model="errorDialog">
          <q-card>
            <q-card-section class="q-pt-md q-pb-md">
              <q-card-title class="text-h5">Please fill out all fields.</q-card-title>
            </q-card-section>
  
            <q-card-actions align="right">
              <q-btn label="Close" color="primary" @click="closeDialog" />
            </q-card-actions>
          </q-card>
        </q-dialog>

      </q-form>

    </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'IndexPage',
  data() {
    return {
      fragment: "",
      pseudonym: "",
      successDialog: false,
      errorDialog: false
    }
  }, 
  computed: {
    formattedData() {
      return this.fragment.replace(/\n/g, "<br>")
    },
  },
  methods: {

    addFragment() {

      if (this.fragment !== "" && this.pseudonym !== "") {

        let value = {
          fragment: this.fragment,
          pseudonym: this.pseudonym,
          date: Date.now()
        }
        this.$axios.post("http://localhost:4000/fragments", value)
        .then(response => {
          this.valueId = response.data.id
          this.showSuccessDialog()
        }
        )
        .catch(error => {
          console.error("Error sending data:", error)
        })

      } else {

        this.showErrorDialog()
      
      }
    },
    showSuccessDialog() {
      this.successDialog = true
    },
    showErrorDialog() {
      this.errorDialog = true
    },
    closeDialog() {
      this.successDialog = false
      this.errorDialog = false
    },
  }
})
</script>

<style lang="sass">

.fragment, .about
  max-width: 500px
  border: 0
  background-color: #f5f5f5
  box-shadow: 10px 10px 10px black

.fragment-text
  white-space: pre-line

</style>