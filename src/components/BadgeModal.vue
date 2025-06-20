<template>
	<div class="modal-overlay" @click="handleOverlayClick">
		<div :class="slugify(sportName)" class="modal-content" @click.stop>
			<div class="modal-header">
				<h2>{{ leagueName }}</h2>
				<button class="close-btn" @click="$emit('close')">
					<svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
						<line x1="18" y1="6" x2="6" y2="18"></line>
						<line x1="6" y1="6" x2="18" y2="18"></line>
					</svg>
				</button>
			</div>

			<div class="modal-body">
				<div v-if="badgeList.length" class="badge-container">
					<ul class="badge-list">
						<li
							v-for="badge in badgeList"
							:key="badge.strSeason"
							class="badge-list-item"
						>
							<div class="date">{{ badge.strSeason }}</div>
							<img
								:src="badge.strBadge"
								:alt="`${leagueName} badge`"
								class="badge-image"
							/>
						</li>
					</ul>
				</div>
				<div v-else class="no-badge">
					<p>No badge available for this league.</p>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: "BadgeModal",
	props: {
		badgeList: {
			type: Object,
			default: null,
		},
		leagueName: {
			type: String,
			required: true,
		},
		sportName: {
			type: String,
			required: true,
		},
	},
	emits: ["close"],

	mounted() {
		document.addEventListener("keydown", this.handleEscKey);
		console.log(this.badgeList);
	},
	beforeUnmount() {
		document.removeEventListener("keydown", this.handleEscKey);
	},
	methods: {
		handleOverlayClick() {
			this.$emit("close");
		},
		handleEscKey(event) {
			if (event.key === "Escape") {
				this.$emit("close");
			}
		},

		slugify(str) {
			return str.toLowerCase().replace(/\s+/g, "-");
		},
	},
};
</script>

<style scoped>
.modal-overlay {
	position: fixed;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	background: rgba(0, 0, 0, 0.5);
	display: flex;
	align-items: center;
	justify-content: center;
	z-index: 1000;
	padding: 1rem;
}

.modal-content {
	background: white;
	border-radius: 0.75rem;
	box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.25);
	max-width: 500px;
	width: 100%;
	max-height: 90vh;
	overflow: hidden;
}

.modal-header {
	display: flex;
	align-items: center;
	justify-content: space-between;
	padding: 1.5rem;
	/* border-bottom: 1px solid #e2e8f0; */
}

.modal-header h2 {
	margin: 0;
	font-size: 1.4rem;
	font-weight: 600;
	background: var(--sporty-soccer);
	color: var(--sporty-text-invert);
	padding: 0.5rem 2rem;
	border-radius: 3rem;
	border-bottom-left-radius: 0;
}

.modal-content.basketball .modal-header h2 {
	background: var(--sporty-basketball);
}
.modal-content.ice-hockey .modal-header h2 {
	background: var(--sporty-ice-hockey);
}
.modal-content.american-football .modal-header h2 {
	background: var(--sporty-american-football);
}
.modal-content.motorsport .modal-header h2 {
	background: var(--sporty-motorsport);
}

.close-btn {
	background: none;
	border: none;
	cursor: pointer;
	padding: 0.5rem;
	color: var(--sporty-text-minimal);
	border-radius: 0.25rem;
	transition: all 0.2s ease;
}

.close-btn:hover {
	color: var(--sporty-text-brand);
}

.close-btn svg {
	width: 1.5rem;
	height: 1.5rem;
}

.modal-body {
	padding: 0 2rem;
}

.badge-container {
	display: flex;
	justify-content: center;
	align-items: center;
	min-height: 200px;
	position: relative;
}

.badge-list {
	display: flex;
	list-style: none;
	padding: 0;
	margin: 40px 0;
	overflow-x: auto;
	gap: 40px;
	scroll-snap-type: x mandatory;
}
.badge-list::before {
	content: "";
	position: absolute;
	top: 40px;
	left: 0;
	width: 100%;
	height: 2px;
	background: var(--sporty-soccer);
}

.modal-content.basketball .badge-list::before {
	background: var(--sporty-basketball);
}
.modal-content.motorsport .badge-list::before {
	background: var(--sporty-motorsport);
}
.modal-content.ice-hockey .badge-list::before {
	background: var(--sporty-ice-hockey);
}
.modal-content.american-football .badge-list::before {
	background: var(--sporty-american-football);
}

.badge-list-item {
	flex: 0 0 auto;
	text-align: center;
	scroll-snap-align: start;
	position: relative;
}

.badge-list-item .date {
	font-weight: bold;
	margin: 10px;
	font-size: 1.1rem;
	color: var(--sporty-text-secondary);
}

.badge-list-item img {
	max-width: 150px;
	object-fit: cover;
}

.badge-list-item::after {
	content: "";
	position: absolute;
	left: 0;
	right: 0;
	margin: 0 auto;
	top: -8px;
	height: 16px;
	width: 16px;
	background: var(--sporty-soccer);
	transform: rotate(45deg);
}

.modal-content.basketball .badge-list-item::after {
	background: var(--sporty-basketball);
}
.modal-content.motorsport .badge-list-item::after {
	background: var(--sporty-motorsport);
}
.modal-content.ice-hockey .badge-list-item::after {
	background: var(--sporty-ice-hockey);
}
.modal-content.american-football .badge-list-item::after {
	background: var(--sporty-american-football);
}

.badge-image {
	max-width: 100%;
	max-height: 300px;
	height: auto;
	border-radius: 0.5rem;
	background: var(--sporty-soccer);
	background: linear-gradient(
		180deg,
		rgba(255, 255, 255, 1) 0%,
		rgba(224, 224, 224, 1) 100%
	);
	padding: 0.5rem;
}

.no-badge {
	text-align: center;
	padding: 2rem;
	color: var(--sporty-text-minimal);
}

.no-badge p {
	margin: 0;
	font-size: 1.125rem;
}

@media (max-width: 768px) {
	.modal-content {
		margin: 1rem;
		max-width: none;
	}

	.modal-header {
		padding: 1rem;
	}

	.modal-body {
		padding: 1rem;
	}
}
</style>
