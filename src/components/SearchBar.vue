<template>
	<div>
		<input
			type="text"
			:value="modelValue"
			@input="$emit('update:modelValue', $event.target.value)"
			placeholder="Search leagues..."
			class="search-input"
		/>
	</div>
</template>

<script>
export default {
	name: "SearchBar",
	props: {
		modelValue: {
			type: String,
			default: "",
		},
	},
	emits: ["update:modelValue"],
};
</script>

<style scoped></style>
