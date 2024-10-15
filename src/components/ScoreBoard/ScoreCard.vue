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
<script lang="ts" setup>
defineProps({
	score: {
		type: Array,
		required: true
	},
	index: {
		type: Number,
		default: 0
	}
})
</script>
<script lang="ts">
export default {
	name: 'ScoreCard'
}
</script>
<style lang="scss" scoped>
@import '@/assets/scss/colors';

.msc-top-score {
	&__card {
		display: flex;
		flex-direction: column;
		gap: 1.25rem;
		width: 100%;
		border-radius: .5rem;
		background-color: transparentize($black, .25);
		padding: 0 2rem 1rem 0;
		// border: 1px solid $white;
		overflow: hidden;

		&__header {
			display: flex;
			align-items: center;
			gap: 1.25rem;
			font-size: 1.5rem;
		}
	}

	&__rank {
		margin-left: -.25rem;
		padding-left: .25rem;
		display: flex;
		justify-content: center;
		align-items: center;
		flex-shrink: 0;
		height: 3rem;
		width: 4rem;
		font-weight: bold;
		background: $red;
		transform: skewX(-15deg);

		span {
			color: $white;
			transform: skewX(15deg);
		}
	}

	&__score {
		display: flex;
		width: 100%;
		font-weight: bold;
		margin-left: 3rem;

		&__stat {
			flex-grow: 1;
			display: flex;
			flex-direction: column;
		}

		&__label {
			font-size: 1rem;
			color: $yellow;
		}

		&__value {
			font-size: 2rem;
			color: $white;
		}
	}

	&__pseudo {
		font-size: 1.5rem;
		font-weight: 800;
		color: #d32b2d;
		text-transform: uppercase;
		white-space: nowrap;
		text-overflow: ellipsis;
		overflow: hidden;
	}

	&__date {
		margin-top: 1rem;
		font-size: .8rem;
		color: lighten($black, 50%);
	}
}
</style>
