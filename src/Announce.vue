<template>
	<div :class="['announce', { '-visible': isVisible }]">
		<button @click="$emit('close')">Z'est fini...</button>
		Nous avons perdu notre 😼 chat noir, <br> il est sûrement avec un Zenika !
		<br><br>
		Échange avec des Zenika pour le retrouver <br>et gagner un T-Shirt !
		<div :class="['timer', { '-end': remaining <= 0 }]">
			{{ minutes }} : {{ secondes }}
		</div>
	</div>
</template>

<script setup lang="ts">
import {computed, ref, watch} from 'vue';

const props = defineProps({
	isVisible: {
		type: Boolean
	}
});

const emits = defineEmits(['close']);

let INTERVAL = 1000; // ms
let DURATION = 10; // ms

const remaining = ref();


const minutes = computed(() => {
	const res = Math.floor(remaining.value / 60);
	if (res < 10)
		return '0' + res;
	return res;
});

const secondes = computed(() => {
	const res = remaining.value % 60;
	if (res < 10)
		return '0' + res;
	return res;
})

watch(() => props.isVisible, (newValue) => {
	if (newValue) {
		remaining.value = DURATION;
		setTimeout(step, INTERVAL);
	}
});


function step() {
		remaining.value -= 1;
		if (remaining.value > 0 && props.isVisible)
			setTimeout(step, INTERVAL);
}

</script>

<style lang="scss" scoped>
@import '@/assets/scss/colors';

.announce {
	position: absolute;
	top: 0;
	left: 0;
	height: 100vh;
	width: 100vw;
	background: linear-gradient(300deg, rgba(191,29,103,1) 0%, rgba(238,34,56,1) 100%);
	color: $white;
	z-index: 1;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: flex-start;
	gap: 3rem;
	padding: 5rem;
	font-family: "Nunito", sans-serif;
	font-weight: 900;
	font-size: 3rem;
	text-transform: uppercase;
	transform: scale(0);
	transform-origin: top right;
	transition: .3s;


	&.-visible {
		transform: scale(1);
	}

	button {
		position: absolute;
		top: 2rem;
		right: 2rem;
		font-family: "Nunito", sans-serif;
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

	.timer {
		font-size: 10rem;
		font-family: "Nunito", sans-serif;

		&.-end {
			animation-duration: 1.2s;
			animation-name: clignoter;
			animation-iteration-count: infinite;
		}
	}
}

@keyframes clignoter {
	0%   { opacity:1; }
	40%   {opacity:0; }
	100% { opacity:1; }
}

</style>
