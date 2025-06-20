<template>
	<div>
		<select
			:value="modelValue"
			@change="$emit('update:modelValue', $event.target.value)"
			class="sport-select"
		>
			<option value="">All Sports</option>
			<option v-for="sport in sports" :key="sport" :value="sport">
				{{ sport }}
			</option>
		</select>
	</div>
</template>

<script>
export default {
	name: "SportFilter",
	props: {
		modelValue: {
			type: String,
			default: "",
		},
		sports: {
			type: Array,
			default: () => [],
		},
	},
	emits: ["update:modelValue"],
};
</script>

<style>
.sporty-filters {
	display: flex;
	align-items: center;
	justify-content: center;
	padding: 1rem 1rem 2rem;
	gap: 1rem;
	background: linear-gradient(
		45deg,
		#1b2631 0%,
		#1b2631 28%,
		#22303f 43%,
		#22303f 48%,
		#1b2631 74%,
		#1b2631 100%
	);
	/* background: radial-gradient(
		circle,
		#1b2631 0%,
		#1b2631 28%,
		#22303f 43%,
		#22303f 48%,
		#1b2631 74%,
		#1b2631 100%
	); */
	flex-direction: row;
}
@media (max-width: 768px) {
	.sporty-filters {
		flex-direction: column;
	}
}
</style>
