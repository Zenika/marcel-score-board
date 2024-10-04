<template>
	<div class="msc-top-score__card">
		<div class="msc-top-score__card__header">
			<p class="msc-top-score__rank"><span>{{ index + 1 }}</span></p>
			<p class="msc-top-score__pseudo">{{ score[0] }}</p>
		</div>
		<div class="msc-top-score__score">
			<div class="msc-top-score__score__stat">
				<p class="msc-top-score__score__label">Bonnes réponses</p>
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
</template>
<script setup>
defineProps({
	score: {
		type: Array,
	},
	index: {
		type: Number,
	}
})
</script>
<script>
export default {
	name: 'ScoreCard'
}
</script>
