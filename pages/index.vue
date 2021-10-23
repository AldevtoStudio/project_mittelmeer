<template>
  <div class="container">
    <br>
    <nuxt-img :src="image" sizes="sm:100vw md:50vw lg:400px" />
    <p class="subtitle">
      {{ dataPoint }}
    </p>
    <div class="buttons" @click="loadNewImage()">
    <button>
      Run Model
    </button>
    </div>
  </div>
</template>

<script>
import unirand from "unirand"
import dummy from "~/static/dummy_image.json"

export default {
  name: 'HomePage',
  
  data() {
    return {
      dataPoint: 'Asís, I ⛳️ You',
      image : '/pareto-6-2.jpeg'
      }
  },

  methods: {
    // Call Model and replace image
    async runModel () {
      console.log("fetching model") // FOR TESTING ONLY


      // get random array using Pareto distribution
      const randomArray = unirand.pareto(1,3).distributionSync(512)
      const trunc = 0.6

      // import ruway module
      const { HostedModel } = require('@runwayml/hosted-models')
      // model authentication
      const model = new HostedModel({
        url: "https://landscapes-favourite-backgrounds-51351e12.hosted-models.runwayml.cloud/v1/",
        token: "Sd04VQa2+dmmNSq0RCxFQQ=="}) // DANGER !! - Needs to be encripted
      // create input object for model
      const inputs = {
        "z": randomArray, 
        "truncation": trunc}
      
      // run model with above data
       model.query(inputs).then(outputs => {
        const modelImage = outputs

        console.log("model runed") // FOR TESTING ONLY
        // replace image from data object
        this.image = modelImage.image
        })



      
    },

    async loadNewImage () {
    this.image = dummy.jojo
    },
  }
}
</script>

<style>

.logo {
  max-width: 300px;
}

.o-button {
  padding: 10px 30px;
}

.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  flex-direction: column;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

</style>
