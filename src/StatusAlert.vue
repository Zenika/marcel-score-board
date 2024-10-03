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
