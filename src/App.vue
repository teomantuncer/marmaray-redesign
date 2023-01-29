<script>
export default {
	data() {
		return {
			date: new Date(),
			stops: ['Gebze', 'Darıca', 'Osmangazi', 'Fatih', 'Çayırova', 'Tuzla', 'İçmeler', 'Aydıntepe', 'Güzelyalı', 'Tersane', 'Kaynarca', 'Pendik', 'Yunus', 'Kartal', 'Başak', 'Atalar', 'Cevizli', 'Maltepe', 'Süreyya Plajı', 'İdealtepe', 'Küçükyalı', 'Bostancı', 'Suadiye', 'Erenköy', 'Göztepe', 'Feneryolu', 'Söğütlüçeşme', 'Ayrılıkçeşmesi', 'Üsküdar', 'Sirkeci', 'Yenikapı', 'Kazlıçeşme', 'Zeytinburnu', 'Yenimahalle', 'Bakırköy', 'Ataköy', 'Yeşilyurt', 'Yeşilköy', 'Florya Akvaryum', 'Florya', 'Küçükçekmece', 'Mustafa Kemal', 'Halkalı'],
			interval: null,
			currentStop: 0
		}
	},
	methods: {
		nextStop() {
			if (this.currentStop === this.stops.length - 1) {
				this.currentStop = 0
			} else {
				this.currentStop++
			}
			setTimeout(() => {
				this.$refs.stops.style.transform = `translateY(${(this.$refs.stops.children[this.currentStop].offsetTop - this.$refs.stops.children[0].offsetTop) * -1}px)`
			}, 2000)
			this.$refs.indicator.style.transform = `translateY(${(this.$refs.stops.children[this.currentStop].offsetTop + 56)}px)`
		}
	},
	mounted() {
		this.interval = setInterval(() => {
			this.date = new Date()
			if (this.date.getSeconds() % 5 === 0) {
				this.nextStop()
			}
		}, 1000)
	},
	beforeUnmount() {
		clearInterval(this.interval)
	}
}
</script>
<template>
	<div>
		<div class="screen">
			<div class="top-right">
				<div class="last-station">
					<span>Son İstasyon:</span>
					<strong>{{ stops[stops.length - 1] }}</strong>
				</div>
				<div class="current-datetime">
					{{ new Intl.DateTimeFormat('tr-TR', { dateStyle: 'short' }).format(date) + ' ' + new Intl.DateTimeFormat('tr-TR', { timeStyle: 'short' }).format(date) }}<br>
					{{ new Intl.DateTimeFormat('tr-TR', { weekday: 'long' }).format(date) }}
				</div>
			</div>
			<ul ref="stops">
				<li ref="indicator" class="indicator"></li>
				<li v-for="(stop, stopIndex) in stops" :class="{ 'is-current': currentStop === stopIndex, 'is-past': currentStop > stopIndex, 'is-next': currentStop + 1 === stopIndex }">{{ stop }}</li>
			</ul>
			<div class="logo">
				<img src="./assets/marmaray.svg" alt="Logo">
			</div>
		</div>
		<a href="https://www.behance.net/gallery/162075609/Marmaray-Redesign-Concept" target="_blank" class="author">
			<img src="https://mir-s3-cdn-cf.behance.net/user/100/e82e07417683837.61bdda33e3b83.jpg" alt="cagkanesnaf">
			<span>Original Design<br> by @cagkanesnaf</span>
		</a>
	</div>
</template>
<style scoped>
.author {
	position: absolute;
	bottom: 0;
	right: 0;
	background-color: #ccc;
	display: flex;
	align-items: center;
	padding: .5rem;
	text-decoration: none;
	color: #000;
	border-radius: 1rem 0 0 0;
}

.author img {
	width: 50px;
	margin-right: .5rem;
	border-radius: 50%;
}

.screen {
	width: 600px;
	height: 450px;
	margin: 0 auto;
	overflow: hidden;
	border-radius: 1rem;
	border: 3px solid var(--color-dark);
	position: relative;
}
@media only screen and (max-width: 600px) {
	.screen {
		transform: scale(.5);
	}
}

.screen .logo {
	position: absolute;
	right: 1rem;
	bottom: 1rem;
	width: 8rem;
}

.screen .top-right {
	position: absolute;
	right: 1rem;
	top: 1rem;
}

.screen .top-right .last-station {
	background-color: var(--color-light);
	margin-bottom: .5rem;
	border-radius: 1rem;
	padding: .5rem 2rem;
	display: flex;
	flex-direction: column;
	align-items: center;
}

.screen .top-right .last-station strong {
	font-size: 1.5rem;
}

.screen .top-right .current-datetime {
	background-color: var(--color-light);
	margin-bottom: .5rem;
	border-radius: 1rem;
	padding: .5rem 2rem;
	display: flex;
	flex-direction: column;
	align-items: center;
	text-align: center;
}

.screen ul {
	list-style: none;
	padding: 0 0 0 3rem;
	margin: 0 0 0 5rem;
	position: relative;
	transition: transform 3s ease;
	transform: translateY(0);
}

.screen ul:before {
	content: '';
	display: block;
	width: 10px;
	height: calc(100% + 100px);
	background: var(--color-primary);
	position: absolute;
	left: 0;
	z-index: 1;
	top: -100px;
}

.screen ul li {
	position: relative;
	margin-top: 2rem;
	font-size: 1.5rem;
	transition: all 3s ease;
}

.screen ul li.is-current {
	font-size: 2rem;
	color: var(--color-secondary);
	font-weight: 700;
	transition-delay: 3s;
}

.screen ul li:before {
	content: '';
	display: block;
	width: 16px;
	height: 16px;
	border-radius: 50%;
	background: var(--color-bg);
	border: 4px solid var(--color-dark);
	position: absolute;
	left: calc(-3rem - 7px);
	z-index: 2;
}

.screen ul li.is-past {
	color: var(--color-past);
}

.screen ul li.is-past:before {
	background: var(--color-secondary);
	z-index: 1;
}

.screen ul li.is-next:after {
	content: 'Gelecek İstasyon';
	font-size: .875rem;
	display: flex;
	align-items: center;
	justify-content: center;
	text-align: center;
	padding: .5rem 2rem .5rem .5rem;
	width: calc(6rem - 2rem);
	height: min-content;
	line-height: 1;
	border-radius: 0 1rem 1rem 0;
	background: var(--color-light);
	position: absolute;
	left: -8rem;
	top: -.5rem;
}

.screen ul li.indicator {
	margin-top: 0;
	position: absolute;
	left: -10px;
	width: 30px;
	height: 30px;
	z-index: 5;
	background-image: url(./assets/indicator.svg);
	background-size: cover;
	transition: transform 3s ease;
	transform: translateY(0);
}

.screen ul li.indicator:before {
	content: '';
	display: block;
	width: 10px;
	height: 200px;
	background: var(--color-secondary);
	position: absolute;
	left: 10px;
	bottom: 25px;
	z-index: -1;
	border-radius: 0;
	border: none;
}
</style>
