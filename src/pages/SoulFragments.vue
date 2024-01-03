<template>
  <q-page class="flex column flex-center justify-center q-px-lg">

    <div class="fragment q-pa-lg q-my-lg" v-for="item in value" :key="item">
        <h5 v-html="item.fragment"></h5>

        <p class="q-mt-md text-h5">Posted by {{ item.pseudonym }} on {{ item.createdAt }}</p>
    </div>
  </q-page>
</template>

<script>
import { DateTime } from 'luxon';
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'SoulFragement',
  data() {
    return {
      value: ""
    }
  },
  
  async created() {

    function shuffleArray(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array;
    }
    const value = await this.$api.get("https://urchin-app-cwm3i.ondigitalocean.app/fragments")
    // console.log(value.data);
    .then(response => {
      const originalData = response.data
      
      let randomizedData = shuffleArray(originalData)

      let formattedData = randomizedData.map((obj) => {

        let time = DateTime.fromISO(obj.createdAt).toFormat('ff')

        console.log(time)

        return {
          ...obj,
          createdAt: time
        }
      })

      this.value = formattedData

    })
    .catch(error => {
      console.error("Error fetching data:", error)
    })
  }
})

</script>

<style lang="sass">

.fragment
  max-width: 500px
  border: 0
  background-color: #f5f5f5
  box-shadow: 10px 10px 10px black
</style>