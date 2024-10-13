<template>
	<div class="speaker">
		<img class="pic" :src="speaker.pic" alt="">
		<div class="content">
			<h2>{{ speaker.name }}</h2>
			<p>{{ speaker.location }}</p>
		</div>
	</div>
</template>

<script setup lang="ts">
defineProps({
	speaker: {
		type: Object,
		required: true,
	}
});
</script>

<style scoped lang="scss">
@import '@/assets/scss/colors';

.speaker {
	display: flex;
	align-items: center;
	gap: 1rem;
	background-color: $white;
	color: $zenika-black;
	border-radius: .75rem;
	padding: .5rem 1.5rem .5rem .5rem;
}

.pic {
	height: 4rem;
	width: 4rem;
	border-radius: .5rem;
}

p {
	color: $zenika-red;
}

</style>
