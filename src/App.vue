<template>
	<button class="announce-btn" @click="isAnnounceVisible = !isAnnounceVisible">Z'est parti !</button>
	<Announce :is-visible="isAnnounceVisible" @close="isAnnounceVisible = false"/>
	<Transition>
		<ScoreBoard v-if="isScoreVisible"/>
		<ConferencePanel :conference="conferences[currentIndex]" v-else/>
	</Transition>
	<div class="loader" v-if="conferences.length > 0"></div>
</template>

<script setup lang="ts">

import {onMounted, ref} from 'vue';
import ScoreBoard from '@/components/ScoreBoard/ScoreBoard.vue';
import Announce from '@/components/Announce.vue';
import ConferencePanel from '@/components/Conference/ConferencePanel.vue';
import conferences  from '@/assets/conferences.json';

const isAnnounceVisible = ref(false);

const currentIndex = ref(0);
const isScoreVisible = ref(true);

const INTERVAL = 20 * 1000;

function increaseIndex() {
	if (currentIndex.value < conferences.length - 1) {
		currentIndex.value ++;
	} else {
		currentIndex.value = 0;
	}
}

function nextSlide() {
	if (isScoreVisible.value) {
		isScoreVisible.value = false;
	} else {
		isScoreVisible.value = true;
		increaseIndex();
	}
	if(conferences.length > 0)
		setTimeout(nextSlide, INTERVAL);
	else
		isScoreVisible.value = true;
}

onMounted(() => {
	// slides rotation
	setTimeout(nextSlide, INTERVAL);
})

</script>

<style scoped lang="scss">
@import '@/assets/scss/colors';

:global(body) {
	overflow: hidden;
}

.v-enter-active,
.v-leave-active {
	transform: translateX(0%);
	transition: 0.3s;
}

.v-enter-from {
	transform: translateX(100%);
}
.v-leave-to {
	transform: translateX(-100%);
}

.announce-btn {
	position: fixed;
	top: 2rem;
	right: 2rem;
	background-color: transparent;
	color: $white;
	outline: none;
	padding: .75rem 1.25rem;
	border: 2px solid $white;
	border-radius: .5rem;
	text-transform: uppercase;
	font-weight: bold;
	transition: .3s;
	cursor: pointer;
	z-index: 1;

	&:hover {
		transform: scale(1.05);
		background-color: $white;
		color: $red;
	}

	&:active {
		transform: scale(1);
		background-color: $white;
		color: $red;
	}
}

@keyframes progress-bar {
	from { width:0 }
	to { width: calc(100vw - .125rem) }
}

.loader {
	position: fixed;
	bottom: .25rem;
	left: .25rem;
	height: .5rem;
	width: 0;
	border-radius: .25em;
	background-color: $white;
	animation: progress-bar 20s infinite linear;
	z-index: 1;
}
</style>
