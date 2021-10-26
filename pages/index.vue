/* 
Js
- default image?
- link to website subpage: info

Css:
- responsive
- crop?

General
- Credits Runway ML
- Hosting: new domain
    vuetify
- QR
- infopage website
- icon

 */

<template>

<body>
  <div>
    <p id="warning" class="subtitle">
      it might take a few seconds awakening the model
    </p>
  </div>
  <div class="container">
    <nuxt-img :src="image" sizes="sm:100vw md:50vw lg:400px"/>
    <!-- <p class="subtitle">
      {{ dataPoint }}
    </p> -->
    

    <!-- #### alte Button Version (mit Run Model Method und) ohne "Hiding" ### -->
    <!-- <div class="buttons" @click="runModel()"> -->
   <!--  <div class="buttons" @click="loadNewImage()">
    <button>
      Run Model
    </button>
    </div> -->

    <div class="buttons" @click="loadNewImage()">
    <button class="ontopbtn" id="btn" @click="hideBtn(), showBtn()">
      generate unique landscape for me
    </button>
    </div>

    <div class="buttons" @click="downloadLandscape()">
      <button class="ontopbtn" id="btnDownload" style="visibility:hidden"> 
        download
    <!-- <a id="btnDownload" style="visibility:hidden" href="/pareto-6-2.jpeg" download="yourbackdrop.jpg">take it home</a> -->
      </button>
    </div>

   <!--  <div class="post">
      <nuxt-img :src="image" title="your individual background" class=center />  
      <div class="post-s">
        <a href="/pareto-6-2.jpeg" download="yourbackdrop.jpg">take it home</a>
      </div>
    </div> -->

  </div>
</body>
</template>

<script>
import unirand from "unirand"
import dummy from "~/static/dummy_image.json"
import runway from "~/static/runway.json"
import { saveAs } from 'file-saver'

export default {
  name: 'remoteness',

  data() {
    return {
      dataPoint: 'Asís, I ⛳️ You',
      image : '/Users/pears/Documents/repos/project_mittelmeer/static/pareto-6-2.jpeg',
      rurl: runway.url,
      rtoken: runway.token,
      title: 'remmoteness - generating landscapes',
      }
  },
  head() {
      return {
        title: this.title,
        meta: [
          // hid is used as unique identifier. Do not use `vmid` for it as it will not work
          {
            hid: 'description',
            content: 'My custom description',
            name: 'viewport',
            content: 'width=device-width initial-scale=1'
          }
        ]
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
        url: this.rurl,
        token: this.rtoken}) // DANGER !! - Needs to be encripted
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

    async loadNewImage() {
    this.image = dummy.jojo
    },

    hideBtn(){
      document.getElementById('btn').style.visibility = 'hidden';
      document.getElementById('warning').style.visibility = 'hidden';
    },

    showBtn(){
      document.getElementById('btnDownload').style.visibility = 'visible';
    },

    downloadLandscape(){
      saveAs(this.image, "mybackdrop.jpg")

    }

  }
}
</script>

<style>

html {
  background-image: linear-gradient(to right, #FED2CF, #fff, #B5D5D8);
}
body {
  padding: 2vw 2vw;
}

nuxt-img {
  display: block;
  margin: 0 auto;
}

/* Container needed to position the button. Adjust the width as needed */
.container {
  position: relative;
  width: 100%;
  border: black;
}

/* Make the image responsive */
.container img {
  width: 100%;
  height: auto;
}

.container .ontopbtn {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  -ms-transform: translate(-50%, -50%);
  background-color: #fff;
  color: rgb(78, 78, 78);
  font-size: 5vw;
  font-family:'Helvetica Neue';
  opacity: .5;
  padding: 12vw 29vw;
  border: none;
  cursor: pointer;
  border-radius: 3px;
}

.container .ontopbtn:hover {
  background-color: #B5D5D8;
  color: black;
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
  margin: 0;
  padding: 2vw 2vw;
  color: black;
  font-size: 5vw;
  text-align: center;
  /* text-shadow: 3px 3px 1px black; */
}

</style>
