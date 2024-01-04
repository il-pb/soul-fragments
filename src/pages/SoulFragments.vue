<template lang="pug">
q-page(class="flex column flex-center justify-center q-px-lg")

  article(class="fragment q-pa-lg q-my-lg" v-for="item in data" :key="item.id")

    h5(v-html="item.fragment")
  
    h6(class="q-mt-md")
    | Posted by {{ item.pseudonym }} on {{ item.createdAt }}      

</template>

<script setup>
import { DateTime } from 'luxon'
import axios from 'axios'
import { onMounted, ref } from 'vue'

let data = ref([])

const getData = () => {
  axios.get("https://urchin-app-cwm3i.ondigitalocean.app/fragments")
  .then(response => {
    const originalData = response.data;

    data.value = originalData.map((obj) => {
      let time = DateTime.fromISO(obj.createdAt).toFormat('ff');

      return {
        ...obj,
        createdAt: time
      }
    })

    return data
    
  })
  .catch(error => {
    console.error("Error fetching data:", error)
  })
}

onMounted(() => {
  getData()

})

</script>



<style lang="sass">

.fragment
  max-width: 500px
  border: 0
  background-color: #f5f5f5
  box-shadow: 10px 10px 10px black
</style>