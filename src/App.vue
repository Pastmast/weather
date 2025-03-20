<template>
	<div class="weather" :class="weatherClass">
		<div class="container">
			<div class="card weather-form">
				<input
					type="text"
					class="weather-form__input"
					v-model="searchQuery"
					@keyup.enter="weatherSearch"
					placeholder="Введите город"
				/>
				<button class="weather-form__btn" @click="weatherSearch">Поиск</button>
			</div>

			<div class="card weather-load" v-if="loading">Загрузка...</div>

			<div class="weather-info" v-show="!error && location && temperature !== 0 && description">
				<div class="card" v-if="error">Ошибка</div>

				<div class="weather-info__text">
					<p class="card">{{ location }}</p>
					<p class="card">{{ temperature }}°С</p>
					<p class="card">{{ description }}</p>
				</div>
			</div>
		</div>

		<div class="weather-bg">
			<div>
				<img class="weather-bg__img bg" src="./assets/bg.jpg" alt="App Background" />
				<img class="weather-bg__img overcast" src="./assets/overcast.jpg" alt="Overcast" />
				<img class="weather-bg__img partly-cloudy" src="./assets/partly-cloudy.jpg" alt="Partly Cloudy" />
				<img class="weather-bg__img sunny" src="./assets/sunny.jpg" alt="Sunny" />
				<img class="weather-bg__img light-rain" src="./assets/light-rain.webp" alt="Light rain" />
				<img class="weather-bg__img light-snow" src="./assets/light-snow.jpg" alt="Light snow" />
				<img class="weather-bg__img mist" src="./assets/mist.jpg" alt="Mist" />
				<img class="weather-bg__img clear-sky" src="./assets/clear-sky.jpg" alt="clear sky" />
			</div>
		</div>
	</div>
</template>

<script>
export default {
	data() {
		return {
			location: '',
			temperature: 0,
			description: '',
			loading: false,
			error: false,
			searchQuery: '',
		};
	},
	computed: {
		weatherClass() {
			if (this.description.includes('Sunny')) {
				return 'sunny';
			} else if (this.description.includes('Overcast')) {
				return 'overcast';
			} else if (this.description.includes('Partly cloudy')) {
				return 'partly-cloudy';
			} else if (this.description.includes('Light rain')) {
				return 'light-rain';
			} else if (this.description.includes('Thunderstorm with light rain')) {
				return 'light-rain';
			} else if (this.description.includes('Light intensity drizzle')) {
				return 'light-rain';
			} else if (this.description.includes('Moderate rain')) {
				return 'light-rain';
			} else if (this.description.includes('Light snow')) {
				return 'light-snow';
			} else if (this.description.includes('Mist')) {
				return 'mist';
			} else if (this.description.includes('Clear')) {
				return 'clear-sky';
			} else {
				return '';
			}
		},
	},
	methods: {
		weatherSearch() {
			this.loading = true;
			this.error = false;
			fetch(`http://api.weatherapi.com/v1/current.json?key=15aa16dfebe54cd19d594143251803&q=${this.searchQuery}`)
				.then((response) => response.json())
				.then((data) => {
					this.loading = false;
					this.location = data.location.name;
					this.temperature = data.current.temp_c;
					this.description = data.current.condition.text;
					this.resetSearchQuery();
				})
				.catch((error) => {
					this.loading = false;
					this.error = true;
					console.error(error);
				});
		},
		resetSearchQuery() {
			this.searchQuery = '';
		},
	},
};
</script>
