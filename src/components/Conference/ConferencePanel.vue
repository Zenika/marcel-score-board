<template>
	<main class="msc-speaker-panel">
		<div class="content">
			<h1 class="title">{{ conference.title }}</h1>
			<div class="tags">
				<div class="tag">
					<span class="material-icons">location_on</span>
					{{ conference.room }}
				</div>
				<div class="tag">
					<span class="material-icons">schedule</span>
					{{ conference.time }}
				</div>
			</div>

			<div class="description">{{ conference.description }}</div>

			<div class="speakers">
				<Speaker v-for="speaker in conference.speakers" :speaker="speaker" :key="speaker.name"/>
			</div>
		</div>
	</main>
</template>

<script setup lang="ts">
import Speaker from './Speaker.vue';

defineProps({
	conference: {
		type: Object,
		required: true
	}
})

</script>

<style scoped lang="scss">
@import '@/assets/scss/colors';

	.msc-speaker-panel {
		position: absolute;
		top: 0;
		left: 0;
		display: flex;
		justify-content: center;
		align-items: center;
		background: linear-gradient(300deg, $zenika-red-2 0%, $zenika-red-1 100%);
		height: 100vh;
		width: 100vw;
		overflow: hidden;

	}

.content {
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: flex-start;
	gap: 2rem;
	max-width: calc(1200px + 4rem);
	padding: 0 2rem;
	color: $white;
	font-family: Nunito, sans-serif;
}

.title {
	font-size: 4rem;
}

.tags {
	display: flex;
	align-items: center;
	gap: 1rem;
}

.tag {
	display: flex;
	align-items: center;
	gap: 0.5rem;
	padding: .25rem 1rem .25rem .5rem;
	border: 2px solid $white;
	border-radius: 1.5rem;
	font-size: 1.25rem;
}

.description {
	margin: 2rem 0;
	font-size: 1.25rem;
	line-height: 2rem;
}

.speakers {
	display: flex;
	align-items: flex-start;
	gap: 2rem;
}
</style>
