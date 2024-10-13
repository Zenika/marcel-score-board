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
</template>

<script setup>
defineProps({
	lastUpdateDate: {
		type: String,
	},
	isUpdating: {
		type: Boolean,
		default: false,
	}
})
</script>
<script>
export default {
	name: 'StatusAlert'
}
</script>
<style lang="scss" scoped>
@import '@/assets/scss/colors';

@keyframes spinning {
	from { transform: rotateZ(0deg) }
	to { transform: rotateZ(360deg) }
}

.msc-updating {
	gap: .75rem;
	position: fixed;
	bottom: 0;
	right: -15rem;
	padding: .75rem 2.5rem 1.25rem 1.75rem;
	background-color: $red;
	color: $white;
	font-family: "Open Sans", sans-serif;
	font-weight: bold;
	z-index: 1;
	transition: .3s;
	transform: skewX(-15deg);

	&.-visible {
		right: -1rem;

	}

	&__content {
		display: flex;
		gap: .75rem;
		align-items: center;
		transform: skewX(15deg);
	}

	&__loader {
		display: block;
		height: 1.5rem;
		width: 1.5rem;
		border: 4px solid saturate($white, 15%);
		border-bottom-color: transparent;
		border-radius: .75rem;
		animation: 1s infinite spinning;
	}
}

.msc-last-update {
	position: fixed;
	bottom: 0;
	right: -15rem;
	padding: .75rem 2.5rem 1.25rem 1.75rem;
	background-color: $red;
	color: $white;
	font-family: "Open Sans", sans-serif;
	font-weight: bold;
	transition: .3s;
	z-index: 1;
	border: 1px solid rgba($black, .05);
	box-shadow: 4px 6px 8px rgba($black, .2);
	transform: skewX(-15deg);

	&.-visible {
		right: -1rem;
	}

	&__content {
		display: flex;
		flex-direction: column;
		align-items: flex-end;
		transform: skewX(15deg);
	}

	&__label {
		font-size: .8rem;
		color: transparentize($white, .25);
	}
}
</style>
