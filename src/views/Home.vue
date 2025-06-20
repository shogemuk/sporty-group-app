<template>
	<div id="app">
		<header>
			<div class="container">
				<h1>
					<span>Sp<span class="highlight">o</span>rts</span>
					<span>Leagues</span>
				</h1>
			</div>
		</header>

		<main class="sporty-main">
			<div class="sporty-filters">
				<SearchBar v-model="searchQuery" />
				<SportFilter
					:sports="availableSports"
					v-model="selectedSport"
				/>
			</div>

			<div v-if="error" class="error">
				<p>{{ error }}</p>
				<button @click="fetchLeagues" class="retry-btn">Retry</button>
			</div>

			<LeagueGrid
				v-else
				:leagues="filteredLeagues"
				:loading-badge="loadingBadge"
				@league-click="handleLeagueClick"
			/>

			<BadgeModal
				v-if="selectedBadge"
				:badgeList="selectedBadge"
				:league-name="selectedLeagueName"
				:sport-name="activeSport"
				@close="closeBadgeModal"
			/>
		</main>
	</div>
</template>

<script>
import { ref, computed, onMounted } from "vue";
import SearchBar from "../components/SearchBar.vue";
import SportFilter from "../components/SportFilter.vue";
import LeagueGrid from "../components/LeagueGrid.vue";
import BadgeModal from "../components/BadgeModal.vue";

export default {
	name: "App",
	components: {
		SearchBar,
		SportFilter,
		LeagueGrid,
		BadgeModal,
	},
	setup() {
		const leagues = ref([]);
		const searchQuery = ref("");
		const selectedSport = ref("");
		const activeSport = ref("");
		const error = ref("");
		const selectedBadge = ref(null);
		const selectedLeagueName = ref("");
		const loadingBadge = ref(null);
		const badgeCache = ref(new Map());

		const availableSports = computed(() => {
			const sports = [
				...new Set(leagues.value.map((league) => league.strSport)),
			];
			return sports.filter(Boolean).sort();
		});

		const filteredLeagues = computed(() => {
			let filtered = leagues.value;

			if (searchQuery.value) {
				// console.log(searchQuery.value);
				const query = searchQuery.value.toLowerCase();
				filtered = filtered.filter(
					(league) =>
						league.strLeague?.toLowerCase().includes(query) ||
						league.strLeagueAlternate?.toLowerCase().includes(query)
				);
			}

			if (selectedSport.value) {
				filtered = filtered.filter(
					(league) => league.strSport === selectedSport.value
				);
			}

			return filtered;
		});

		const fetchLeagues = async () => {
			error.value = "";

			try {
				const response = await fetch(
					"https://www.thesportsdb.com/api/v1/json/3/all_leagues.php"
				);
				const data = await response.json();

				if (data.leagues) {
					leagues.value = data.leagues;
				} else {
					throw new Error("No leagues data received");
				}
			} catch (err) {
				error.value = "Failed to fetch leagues. Please try again.";
				console.error("Error fetching leagues:", err);
			} finally {
			}
		};

		const fetchSeasonBadge = async (leagueId) => {
			// Check cache first
			if (badgeCache.value.has(leagueId)) {
				return badgeCache.value.get(leagueId);
			}

			loadingBadge.value = leagueId;

			try {
				const response = await fetch(
					`https://www.thesportsdb.com/api/v1/json/3/search_all_seasons.php?badge=1&id=${leagueId}`
				);
				const data = await response.json();

				let badgeUrl = null;
				if (data.seasons && data.seasons.length > 0) {
					badgeUrl = data.seasons;
				} else {
					badgeUrl = [{ strSeason: "2025", strBadge: null }];
				}

				// Cache the result
				badgeCache.value.set(leagueId, badgeUrl);
				return badgeUrl;
			} catch (err) {
				console.error("Error fetching badge:", err);
				badgeCache.value.set(leagueId, null);
				return null;
			} finally {
				loadingBadge.value = null;
			}
		};

		const handleLeagueClick = async (league) => {
			const badgeUrl = await fetchSeasonBadge(league.idLeague);
			selectedBadge.value = badgeUrl.filter(
				(item) => item.strBadge !== null
			);
			selectedLeagueName.value = league.strLeague;
			activeSport.value = league.strSport;
		};

		const closeBadgeModal = () => {
			selectedBadge.value = null;
			selectedLeagueName.value = "";
			activeSport.value = "";
		};

		onMounted(() => {
			fetchLeagues();
		});

		return {
			leagues,
			searchQuery,
			selectedSport,
			activeSport,
			error,
			selectedBadge,
			selectedLeagueName,
			loadingBadge,
			availableSports,
			filteredLeagues,
			fetchLeagues,
			handleLeagueClick,
			closeBadgeModal,
		};
	},
};
</script>
<style scoped></style>
