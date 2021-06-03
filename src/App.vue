<template>
  <div class="content">
    <Header header_title="Bird ID" />
    <div class="main">
      <img
        id="loaded_image"
        v-bind:src="this.imageUrl"
        alt=""
      />
      <p v-if="speciesSet" style="marginBottom: 15px; font-size: 22px;">{{ species }}</p>
      <a ref="googleLink" hidden v-bind:href="googleUrl"></a>
      <button v-if="speciesSet" id="googleButton" style="display: flex; flex-direction: row; align-items: center; padding: 10px;" @click="googleLink()">
        <img src="https://storage.googleapis.com/support-kms-prod/ZAl1gIwyUsvfwxoW9ns47iJFioHXODBbIkrK" alt="" width="20" height="20">
        <p style="color: black; font-size: 14px; margin-left: 20px; margin-top: 0px; font-weight: 800;">Bei Google ansehen</p>
      </button>
      <input
        ref="uploadInput"
        type="file"
        name="filename"
        hidden
        @change="fileNameChanged()"
      />
      <button @click="uploadImage()" v-if="!spinnerActive">Bild hochladen</button>
      <h3 style="margin-bottom: 10px; margin-top: 10px;">{{ fileName }}</h3>
      <button style="margin-bottom: 20px;" v-if="runClassify && !spinnerActive" @click="classify()">Klassifizieren</button>
      <div v-if="spinnerActive">
        <img id="spinner" src="https://www.bluvale.com/skin/frontend/bluvale/default/images/more/loader/loader-white1.gif" alt="" height="40" width="40">
      </div>
    </div>
  </div>
</template>

<script>
import Header from "./components/Header.vue";

export default {
  name: "App",
  data() {
    return {
      species: "",
      fileName: "(Keine Datei hochgeladen)",
      speciesSet: false,
      googleUrl: "",
      runClassify: false,
      spinnerActive: false,
      imageUrl: "https://images.pexels.com/photos/349758/hummingbird-bird-birds-349758.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260"
    };
  },
  components: {
    Header,
  },
  methods: {
    uploadImage() {
      console.log("Hochladen!");
      this.$refs.uploadInput.click();
    },
    classify() {
      if (this.$refs.uploadInput.files[0] !== undefined) {
        this.spinnerActive = true;
        var blobFile = this.$refs.uploadInput.files[0];
        fetch("http://192.168.178.88:5000/classify", {
          method: "POST",
          body: blobFile,
        }).then((res) => {
          res.json().then((data) => {
            this.species = data["some_key"];
            this.speciesSet = true;
            this.art = data["some_key"];
            this.spinnerActive = false;
            this.fileName = "(Keine neue Datei hochgeladen)";
            this.runClassify = false;
            let encodedSpeciesGoole = this.art.replace(" ", "+");
            this.googleUrl = "https://www.google.de/search?q="+encodedSpeciesGoole+"&source=lnms&tbm=isch"
          });
        });
      } else {
        this.fileName = "Es wurde kein Bild gefunden.";
      }
    },
    fileNameChanged() {
      if (this.$refs.uploadInput.files[0] !== undefined) {
        this.runClassify = true;
        this.speciesSet = false;
        var blobFile = this.$refs.uploadInput.files[0];
        this.fileName = blobFile.name;
        var url = (window.URL || window.webkitURL).createObjectURL(blobFile);
        this.imageUrl = url;
      } else {
        this.fileName = "(Keine Datei hochgeladen)";
      }
    },
    googleLink() {
      this.$refs.googleLink.click();
    }
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Manrope:wght@200;300;800&display=swap");

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

h3 {
  color: white;
  font-weight: 200;
  font-size: 14px;
}

.content {
  font-family: Manrope;
  background-color: white;
}

.main {
  background: linear-gradient(150deg, #a681ff 0%, #ff3e3e 80%);
  border-radius: 6px;
  box-shadow: 0px 10px 30px 0px rgba(0, 0, 0, 0.2);
  margin-top: 2em;
  margin-left: 10px;
  margin-right: 10px;
  height: auto;
  text-align: center;
  display: flex;
  flex-direction: column;
}

button {
  padding: 10.5px 30px;
  box-sizing: border-box;
  border-radius: 32px;
  font-family: manrope;
  font-size: 20px;
  background: rgba(255, 255, 255, 1);
  box-shadow: 0px 10px 30px 0px rgba(0, 0, 0, 0.3);
  border: none;
  color: rgb(0, 0, 0);
  margin: 10px;
  margin-top: 20px;
  margin-bottom: 10px;
  width: 300px;
  margin-right: auto;
  margin-left: auto;
}

button:hover {
  color: rgb(0, 0, 0);
}

button:active {
  color: white;
}

h2 {
  text-align: center;
  padding-top: 20px;
  color: white;
  font-weight: 800;
}

#loaded_image {
  width: 200px;
  margin-right: auto;
  margin-left: auto;
  margin-top: 30px;
  border-radius: 10px;
}

p {
  color: white;
  font-weight: 800;
  margin-top: 20px;
}

#spinner {
  margin-right: auto;
  margin-left: auto;
  margin-bottom: 20px;
  margin-top: 10px;  
}

#googleButton {
  width: 205px;
}
</style>
