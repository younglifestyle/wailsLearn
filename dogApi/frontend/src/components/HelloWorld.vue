<script setup>
import {onMounted, reactive, ref} from 'vue'
import {FetchAnyBreed, FetchByBreed, Greet, SelectBreed} from '../../wailsjs/go/main/App'

const data = reactive({
  name: "",
  resultText: "Please enter your name below 👇",
})

const responseObject = ref({});
const breeds = ref([]);
const photos = ref([]);
const selectedBreed = ref('');
const showRandomPhoto = ref(false);
const showBreedPhotos = ref(false);

function init() {
  selectBreed();
}

onMounted(() => {
  selectBreed();
});

function fetchAnyBreed() {
  showRandomPhoto.value = false;
  showBreedPhotos.value = false;
  FetchAnyBreed().then((result) => {
    responseObject.value = result;

    showRandomPhoto.value = true;
  });
}

function selectBreed() {
  SelectBreed().then((result) => {
    breeds.value = result;
  });
}

function fetchByBreed() {
  init();
  showRandomPhoto.value = false;
  showBreedPhotos.value = false;
  FetchByBreed(selectedBreed.value).then((result) => {
    photos.value = result;
    showBreedPhotos.value = true;
  });
}

function greet() {
  Greet(data.name).then(result => {
    data.resultText = result
  })
}

</script>

<template>
  <main>
    <h3>Dogs API</h3>
    <div>
      <button class="btn" @click="fetchAnyBreed">Fetch a dog randomly</button>
      &nbsp;&nbsp;&nbsp; Click on down arrow to select a breed
      <select v-model="selectedBreed">
        <option v-for="breed in breeds" :value="breed" :key="breed">{{ breed }}</option>
      </select>
      <button class="btn" @click="fetchByBreed">Fetch by this breed</button>
    </div>
    <br />
    <img
        v-if="showRandomPhoto"
        id="random-photo"
        :src="responseObject.message"
        alt="No dog found"
    />
    <div v-if="showBreedPhotos">
      <img
          v-for="photo in photos"
          :key="photo"
          id="breed-photos"
          :src="photo"
          alt="No dog found"
      />
    </div>
<!--    <div id="result" class="result">{{ data.resultText }}</div>-->
<!--    <div id="input" class="input-box">-->
<!--      <input id="name" v-model="data.name" autocomplete="off" class="input" type="text"/>-->
<!--      <button class="btn" @click="greet">Greet</button>-->
<!--    </div>-->
  </main>
</template>

<style scoped>
#random-photo {
  width: 600px;
  height: auto;
}

#breed-photos {
  width: 300px;
  height: auto;
}

.btn:focus {
  border-width: 3px;
}

.result {
  height: 20px;
  line-height: 20px;
  margin: 1.5rem auto;
}

.input-box .btn {
  width: 60px;
  height: 30px;
  line-height: 30px;
  border-radius: 3px;
  border: none;
  margin: 0 0 0 20px;
  padding: 0 8px;
  cursor: pointer;
}

.input-box .btn:hover {
  background-image: linear-gradient(to top, #cfd9df 0%, #e2ebf0 100%);
  color: #333333;
}

.input-box .input {
  border: none;
  border-radius: 3px;
  outline: none;
  height: 30px;
  line-height: 30px;
  padding: 0 10px;
  background-color: rgba(240, 240, 240, 1);
  -webkit-font-smoothing: antialiased;
}

.input-box .input:hover {
  border: none;
  background-color: rgba(255, 255, 255, 1);
}

.input-box .input:focus {
  border: none;
  background-color: rgba(255, 255, 255, 1);
}
</style>
