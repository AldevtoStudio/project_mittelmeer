<template>

<body>
  <div>
    <p id="intro" class="subtitle">
      A unique landscape, an ideal background can be generated for you.
    <br>
    </p>
</div> 
 
  <div class="container">
    <nuxt-img :src="image" sizes="sm:100vw md:50vw lg:200px"/>
    <!-- <img src="project_mittelmeer/static/leer.png" /> -->
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

    <div class="buttons" @click="runModel()">
    <button class="ontopbtn" id="btn" @click="hideBtn(), showBtn()">
      generate
    </button>
    </div>

    <div class="buttons" @click="downloadLandscape()">
      <button class="ontopbtn" id="btnDownload" style="visibility:hidden"> 
        download
    <!-- <a id="btnDownload" style="visibility:hidden" href="/pareto-6-2.jpeg" download="yourbackdrop.jpg">take it home</a> -->
      </button>
    </div>

  <div>
    <p id="warning" class="subtitle">
      💤💤💤 <br> The model might be asleep, <br> it can take a few seconds to run. <br> 💤💤💤
    </p>
  </div>

  <div class="infolink">
    <a href="http://start.johannamichel.com/remoteness-insularity-difficult-topography/">info</a>
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
import dummy1 from "~/static/dummy_image1.json"
import runway from "~/static/runway.json"
import { saveAs } from 'file-saver'

export default {
  name: 'remoteness',

  data() {
    return {
      image: dummy1.default,
      rurl: runway.url,
      rtoken: runway.token,
      title: 'remoteness - generator',
      }
  },
  head() {
      return {
        title: this.title,
        meta: [
          {
            content: 'My custom description',
            name: 'viewport',
            content: 'width=device-width initial-scale=1'
          }
        ]
      }
    },

  methods: {
    // Call Model and replace image
    async runModel() {
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

   /*  async hide() {
      document.getElementById('btn').style.visibility = 'hidden';
      document.getElementById('intro').style.visibility = 'hidden';
      document.getElementById('warning').style.visibility = 'hidden';
    }, */

    hideBtn() {
      
      setTimeout(function() {document.getElementById('intro').style.visibility = 'hidden';}, 1000);
      setTimeout(function() {document.getElementById('btn').style.visibility = 'hidden';}, 1000);
      setTimeout(function() {document.getElementById('warning').style.visibility = 'hidden';}, 10000);
    },

    showBtn(){
      setTimeout(function() {document.getElementById('btnDownload').style.visibility = 'visible';}, 1500);
    },

    downloadLandscape(){
      if (this.image === dummy1.default) {
        alert("The model has been asleep, it has not generated your landscape yet. Sometimes it can take up to 2 min. If it takes too long, just refresh the page in 2-5 min, when the model is awake. ⏰");
      } else {
      saveAs(this.image, "my_remote_backdrop.jpg")
      }
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
  max-width: 100%;
  height: auto;
  padding: 3vw 0 0 0;
}

.container .ontopbtn {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  -ms-transform: translate(-50%, -50%);
  background-color: #B5D5D8;
  color: black;
  font-size: clamp(16px, 3vw, 22px);
  font-style: italic;
  text-decoration: underline;
  font-family: "Times New Roman", Times, serif;
  opacity: .5;
  padding: 2vw 4vw;
  border: none;
  cursor: pointer;
  border-radius: 1px;
}

.container .ontopbtn:hover {
  background-color: #fff;
}

.m-fadeOut {
  visibility: hidden;
 /*  opacity: 0;
  transition: visibility 0s linear 300ms, opacity 300ms; */
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

.infolink {
  padding: 3vw 0 0 0;
  text-align: right; 
  color: red;
  font-size: clamp(16px, 3vw, 22px);
}

.subtitle {
  margin: 0;
  padding: 2vw 2vw;
  color: black;
  font-size: clamp(16px, 3vw, 22px);
  text-align: center;
  /* text-shadow: 3px 3px 1px black; */
}

</style>
