<template>
	<div
		class="league-item sporty-border"
		@click="handleClick"
		:class="({ loading: isLoading }, slugify(league.strSport))"
	>
		<div class="card-header">
			<h3 class="league-name" v-html="highlightO(league.strLeague)"></h3>
			<span class="sport-badge">{{ league.strSport }}</span>
		</div>

		<div class="card-body">
			<p v-if="league.strLeagueAlternate" class="alternate-name">
				Also known as: {{ league.strLeagueAlternate }}
			</p>

			<div class="card-footer">
				<button class="view-badge-btn" :disabled="isLoading">
					<span v-if="isLoading">Loading...</span>
					<span v-else>View League Badges</span>
				</button>
			</div>
		</div>

		<div v-if="isLoading" class="loading-overlay">
			<div class="spinner"></div>
		</div>
	</div>
</template>

<script>
export default {
	name: "LeagueItem",
	props: {
		league: {
			type: Object,
			required: true,
		},
		isLoading: {
			type: Boolean,
			default: false,
		},
	},
	emits: ["click"],
	methods: {
		handleClick() {
			if (!this.isLoading) {
				this.$emit("click");
			}
		},
		slugify(str) {
			return str.toLowerCase().replace(/\s+/g, "-");
		},
		highlightO(str) {
			if (!str) return "";
			return str.replace(/o/gi, '<span class="highlight">$&</span>');
		},
	},
};
</script>

<style scoped>
.league-item {
	box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
	transition: all 0.2s ease;
	cursor: pointer;
	position: relative;
	overflow: hidden;
	background: linear-gradient(
		135deg,
		var(--sporty-surface-brand) 0%,
		/* var(--sporty-surface-brand) 30%, */ var(--sporty-text-disabled) 100%
	);
	display: flex;
	flex-direction: column;
	justify-content: space-between;
}

/* .league-item::before {
	content: "";
	position: absolute;
	top: -50%;
	right: -50%;
	width: 100px;
	height: 100px;
	background: radial-gradient(
		circle,
		rgba(255, 107, 53, 0.1) 0%,
		transparent 70%
	);
	border-radius: 50%;
	pointer-events: none;
} */

.league-item:hover {
	transform: translateY(-2px);
	box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
}

.league-item.loading {
	cursor: not-allowed;
}

.card-header {
	padding: 1.5rem 1.5rem 1rem;
	/* border-bottom: 1px solid #f1f5f9; */
}

.league-name {
	font-size: 1.25rem;
	font-weight: 600;
	color: var(--sporty-text-secondary);
	margin: 0 0 0.5rem 0;
	line-height: 1.4;
}

.sport-badge {
	display: inline-block;
	background: var(--sporty-soccer);
	color: white;
	padding: 0.25rem 0.75rem;
	border-radius: 1rem;
	border-bottom-left-radius: 0;
	font-size: 0.875rem;
	font-weight: 500;
}
.motorsport .sport-badge {
	background-color: var(--sporty-motorsport);
}
.ice-hockey .sport-badge {
	background-color: var(--sporty-ice-hockey);
}
.basketball .sport-badge {
	background-color: var(--sporty-basketball);
}
.american-football .sport-badge {
	background-color: var(--sporty-american-football);
}

.card-body {
	padding: 1rem 1.5rem 1.5rem;
}

.alternate-name {
	color: var(--sporty-text-secondary);
	font-size: 0.875rem;
	margin: 0 0 1rem 0;
	font-style: italic;
}

.card-footer {
	margin-top: 1rem;
}

.view-badge-btn {
	width: 100%;
	background: var(--sporty-text-minimal);
	/* background: #048ba8; */
	border: none;
	padding: 0.75rem 1rem 0.75rem 3rem;
	font-family: "Poppins", serif;
	letter-spacing: 1px;
	border-radius: 3rem;
	font-size: 0.875rem;
	color: var(--sporty-text-invert);
	cursor: pointer;
	transition: all 0.2s ease;
	position: relative;
	font-weight: 700;
	text-transform: uppercase;
	text-align: left;
}

.view-badge-btn:before {
	content: "";
	background-image: url(assets/img/arrow.svg);
	background-size: 100%;
	width: 18px;
	height: 10px;
	position: absolute;
	left: 1rem;
	top: 50%;
	transform: translateY(-50%);
	filter: invert(1);
}

.view-badge-btn:hover:not(:disabled) {
	background: var(--sporty-text-brand);
}

.view-badge-btn:disabled {
	opacity: 0.6;
	cursor: not-allowed;
}

.loading-overlay {
	position: absolute;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	background: rgba(255, 255, 255, 0.8);
	display: flex;
	align-items: center;
	justify-content: center;
}

.spinner {
	width: 2rem;
	height: 2rem;
	border: 3px solid var(--sporty-surface-secondary);
	border-top: 3px solid var(--sporty-text-brand);
	border-radius: 50%;
	animation: spin 1s linear infinite;
}

@keyframes spin {
	0% {
		transform: rotate(0deg);
	}
	100% {
		transform: rotate(360deg);
	}
}
</style>
