<script setup lang="ts">
import {ref} from 'vue'

const text = ref("");
const textSpeech = ref("");

defineProps<{ msg: string }>()


function playSpeechSynthesis() {
  const SpeechRecognitionClass =   webkitSpeechRecognition;
  const recognition = new SpeechRecognitionClass();

  recognition.lang = 'fr-Fr';
  recognition.interimResults = false;

  // event current voice reco word
  recognition.addEventListener("result", event => {

    const text = Array.from(event.results)
        .map(result => result[0])
        .map(result => result.transcript)
        .join("");
    textSpeech.value = text;
    console.log(text)

  });

  // end of transcription
  recognition.addEventListener("end", () => {
    recognition.stop();
  });
  recognition.start();
}

function synthesis() {
  playSpeechSynthesis();
  const utterance = new SpeechSynthesisUtterance(text.value);
  utterance.lang = 'fr-Fr';
  speechSynthesis.speak(utterance);
}
</script>

<template>
  <h1>{{ msg }}</h1>

  <div class="card">

    <input v-model="text" type="text"/>
    <input v-model="textSpeech" type="text"/>
    <button type="button" @click="synthesis">SPeech</button>

  </div>

</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
