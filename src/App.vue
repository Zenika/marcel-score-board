<template>
  <div class="msc-updating" :class="{ '-visible': isUpdating }">
    <span class="msc-updating__loader"></span>
    Mise à jour en cours...
  </div>
  <div class="msc-last-update" :class="{ '-visible': !isUpdating }">
    <span class="msc-last-update__label">Dernière mise à jour:</span>
    {{ lastUpdateDate }}
  </div>
  <main class="msc-page">
    <h1 class="msc-title">Tableau des scores</h1>
    <p class="msc-subtitle" v-if="scores.length === 0">
      Aucun score à afficher !
    </p>
    <div class="msc-top-score" v-if="scores.length > 0">
      <div class="msc-top-score__card" v-for="(score, index) in scores.slice(0, 3)" :key="score[0]">
        <p class="msc-top-score__rank"> {{ index + 1 }}</p>
        <div class="msc-top-score__content">
          <p class="msc-top-score__score">
            {{ (score[2]) }}
            <span class="msc-top-score__score__pts">pts</span>
          </p>
          <p class="msc-top-score__pseudo">{{ score[1] }}</p>
          <p class="msc-top-score__date">{{ formatDate(score[0]) }}</p>
        </div>
      </div>
    </div>
    <table class="msc-score-table" v-if="scores.length > 3">
      <thead>
        <tr>
          <th>n°</th>
          <th>Score</th>
          <th>Pseudo</th>
          <th>Date</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(score, index) in scores.slice(3, 10)" :key="score[0]">
          <td>
            <span class="msc-score-table__rank">{{ index + 4 }}</span>
          </td>
          <td class="msc-score-table__score">
            {{ score[2] }}
            <span class="msc-score-table__score__pts">pts</span>
          </td>
          <td>{{ score[1] }}</td>
          <td class="msc-score-table__date" v-html="formatDate(score[0]).split(' ').join('<br>')">
          </td>
        </tr>
      </tbody>
    </table>
  </main>
</template>
<script setup lang="ts">
import {onMounted, ref} from "vue";

const isUpdating = ref<boolean>(true);
const lastUpdateDate = ref<string>("");
let scores = ref<Array<Array<string>>>([]);

const SPREADSHEET_ID: string = "1G9simK3R8ByIOrFMhmcDfon4GF-70_stKzjdztV5r5Q";

async function loadSpreadSheet(apiKey: any): Promise<void> {
  isUpdating.value = true;
  const response = await fetch(`https://sheets.googleapis.com/v4/spreadsheets/${SPREADSHEET_ID}/values/A2:D?key=${apiKey}`, {
    method: "GET",
  });
  const data = await response.json();
  isUpdating.value = false;
  if (data.values)
    updateScore(data.values);
  else
    updateScore([]);
}

function updateScore(values: Array<Array<string>>) {
  lastUpdateDate.value = formatDate(Date.now().toLocaleString());
  scores.value = values;
  scores.value.sort((a, b) => {
    if (parseInt(a[2]) < parseInt(b[2]))
      return 1;
    return -1;
  });
  localStorage.setItem("scores", JSON.stringify(scores.value));
}

function formatDate(value: string): string {
  const now = new Date(value);
  const date = now.toLocaleDateString('fr');
  const hour = now.toLocaleTimeString('fr').split(":").slice(0, 2).join(":");
  return `${date} ${hour}`;
}

onMounted(() => {
  // last data used
  const oldScores = localStorage.getItem("scores");
  if (oldScores)
    scores.value = JSON.parse(oldScores);

  // update data
  window.addEventListener('message', event => {
    if (event.source !== parent) return
    const message = event.data

    if (message.type === 'propsChange') {
      const { newProps } = message.payload;
      loadSpreadSheet(newProps.apiKey);
      // mettre à jour l'affichage du plugin en utilisant newProps
    }
  });

  window.parent.postMessage({ type: 'loaded' }, '*');
});

</script>
