<template lang="pug">
q-page(class="flex column flex-center justify-center q-px-lg")

  div(class="about q-pa-lg q-mt-lg")

    h4
      | Soul Fragments is a digital sanctuary where your secrets find a voice in the vast expanse of the internet. Unburden your soul and unveil the depths of your untold stories here.

  div(class="fragment q-pa-lg q-my-lg")

    p(class="text-h4 q-mx-auto")
      | Send a little fragment of your soul to the world.

    q-form(@submit="addFragment" class="q-gutter-md")

      q-input(class="fragment-text text-h5" v-model="fragment" filled :rows="20" type="textarea" placeholder="Your soul fragment")

      q-input(class="text-h5" v-model="pseudonym" filled placeholder="Your pseudonym")

      q-btn(v-if="fragment ==='' && pseudonym ===''" class="q-mb-" label="Submit" type="submit" color="primary" disable)

      q-btn(v-else class="q-mb-md" label="Submit" type="submit" color="primary")
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'
import { useQuasar } from 'quasar';

let fragment = ref("")
let pseudonym = ref("")
const $q = useQuasar()

const formattedData = () => fragment.value.replace(/\n/g, '<br>')

const addFragment = () => {

  formattedData()

  let value = {
    fragment: fragment.value,
    pseudonym: pseudonym.value,
    date: Date.now()
  }
  
  axios.post("https://urchin-app-cwm3i.ondigitalocean.app/fragments", value)
  
  .then(response => {

    fragment.value = ""
    pseudonym.value = ""

    showNotif()
    
  })
  .catch(error => {
    console.error("Error sending data:", error)
  })

}

const showNotif = () => {
  $q.notify({
    message: 'Thank you sharing a soul fragment!',
    color: 'white',
    textColor: 'purple-8'
  })
}



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