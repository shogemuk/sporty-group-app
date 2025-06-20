<template>
	<div class="league-grid">
		<div v-if="leagues.length === 0" class="no-results">
			<p>No leagues found matching your criteria.</p>
		</div>

		<div v-else class="grid">
			<LeagueItem
				v-for="league in leagues"
				:key="league.idLeague"
				:league="league"
				:is-loading="loadingBadge === league.idLeague"
				@click="$emit('league-click', league)"
			/>
		</div>
	</div>
</template>

<script>
import LeagueItem from "./LeagueItem.vue";

export default {
	name: "LeagueGrid",
	components: {
		LeagueItem,
	},
	props: {
		leagues: {
			type: Array,
			default: () => [],
		},
		loadingBadge: {
			type: String,
			default: null,
		},
	},
	emits: ["league-click"],
};
</script>

<style scoped>
.league-grid {
	/* margin-top: 2rem; */
}

.grid {
	display: grid;
	grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
	gap: 1.5rem;
	padding: 0 1rem 2rem;
}

/* Tablet and up to small desktops: 2 columns */
@media (max-width: 768px) {
	.grid {
		grid-template-columns: 1fr 1fr;
		gap: 1rem;
	}
}

/* Mobile portrait: 1 column */
@media (max-width: 480px) {
	.grid {
		grid-template-columns: 1fr;
	}
}

.no-results {
	text-align: center;
	padding: 3rem;
	color: var(--sporty-text-secondary);
}

.no-results p {
	font-size: 1.125rem;
	margin: 0;
}
</style>
