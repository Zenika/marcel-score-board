<template>
  <div class="msc-updating" :class="{ '-visible': isUpdating }">
	  <div class="msc-updating__content">
	    <span class="msc-updating__loader"></span>
	    Mise à jour en cours...
	  </div>
  </div>
  <div class="msc-last-update" :class="{ '-visible': !isUpdating }">
	  <div class="msc-last-update__content">
		  <span class="msc-last-update__label">Dernière mise à jour :</span>
		  {{ lastUpdateDate }}
	  </div>
  </div>
	<div class="msc-zenika-logo"></div>
  <main class="msc-page">
	  <h2 class="msc-title">Tableau des scores</h2>
    <p class="msc-subtitle" v-if="scores.length === 0">
      Aucun score à afficher !
    </p>
    <div class="msc-top-score" v-if="scores.length > 0">
      <div class="msc-top-score__card" v-for="(score, index) in scores.slice(0, 3)" :key="score[0]">
        <div class="msc-top-score__card__header">
	        <p class="msc-top-score__rank"> <span>{{ index + 1 }}</span></p>
	        <p class="msc-top-score__pseudo">{{ score[0] }}</p>
        </div>
	      <div class="msc-top-score__score">
		      <div class="msc-top-score__score__stat">
			      <p class="msc-top-score__score__label">Score</p>
			      <p class="msc-top-score__score__value">
				      {{ score[3] }}
			      </p>
		      </div>
		      <div class="msc-top-score__score__stat">
			      <p class="msc-top-score__score__label">Temps</p>
			      <p class="msc-top-score__score__value">
				      {{ score[2] }}
			      </p>
		      </div>
	      </div>
      </div>
    </div>
    <table class="msc-score-table" v-if="scores.length > 3">
      <thead>
        <tr>
          <th>Username</th>
          <th>Score</th>
          <th>Temps</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(score, index) in scores.slice(3, 10)" :key="score[0]">
	        <td>
		       <div class="msc-score-table__pseudo">
			       <div class="msc-score-table__rank">
				       <span>{{ index + 4 }}</span>
			       </div>
			       {{ score[0] }}
		       </div>
	        </td>
          <td>{{ score[3] }}</td>
	        <td> {{ score[2] }}</td>
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

const SPREADSHEET_ID: string = "1U5A97xU_V6hAa4p2_pvPZQL4552lRpqSObL5H1CB9oI";
const REFRESH_RATE: number = 60 * 5;

async function loadSpreadSheet(apiKey: any): Promise<void> {
  isUpdating.value = true;
  const response = await fetch(`https://sheets.googleapis.com/v4/spreadsheets/${SPREADSHEET_ID}/values/A2:E?key=${apiKey}`, {
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
  const date = new Date(Date.now())
  lastUpdateDate.value = formatDate(date.toString());
  scores.value = values;
  scores.value.sort(sortScore);
  localStorage.setItem("scores", JSON.stringify(scores.value));
}

function sortScore(a:Array<string>, b: Array<string>) {
	if (parseInt(a[3]) < parseInt(b[3]))
		return 1;
	if (parseInt(a[3]) === parseInt(b[3]))
		return a[2] < b[2] ? 1 : -1;
	return -1;
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
	loadSpreadSheet("AIzaSyCasDc28gfYMH1La9vxH3IMfHYgM_Yq8Rk");
	setInterval(() => loadSpreadSheet("AIzaSyCasDc28gfYMH1La9vxH3IMfHYgM_Yq8Rk"), REFRESH_RATE * 1000)

  window.parent.postMessage({ type: 'loaded' }, '*');
});

</script>
