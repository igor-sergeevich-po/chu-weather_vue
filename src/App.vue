<script>
import axios from 'axios';

export default {
	data() {
		return {
			town: "",
			info: null,
			error: ""
		}
	},
	computed: {
		cityName() {
			return '" ' + this.town + ' "';
		},
		showTemp() {
			return 'Температура:' + this.info.main.temp
		},
		showMinTemp() {
			return 'Минимальная:' + this.info.main.temp_min
		},
		showMaxTemp() {
			return 'Максимальная:' + this.info.main.temp_max
		},
		showFeelsLike() {
			return 'Ощущается как:' + this.info.main.feels_like
		}
	},
	methods:{

		getWeather() {
			if(this.town.trim().length<2) {
				this.error = 'Слишком мало символов :(';
				return false;
			}
			this.error = "";

			axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.town}&appid=a4f099f0b6b099baad5e4a49c962bb64`)
			.then(resp => this.info = resp.data)
			.catch(err => this.error = err.message)
			
		}
	},

}
</script>

<template>
	<div className="wrapper">
		<h1>Прогноз погоды Чу!</h1>
		<p>Узнать прогноз в {{ town === ""? "вашем городе?" : ":"+town  }}</p>
		<input v-on:input="this.town = $event.target.value" type="text" placeholder="введите город">
		<button v-show="town!==''" @click="getWeather">получить</button>
		<p v-show="error!==''" className="error">{{ error }}</p>
		<p v-show="info !== null">	{{ info?.main?.temp }}</p>
	</div>
</template>

<style scoped>
.wrapper{
	width: 700px;
	height: 500px;
	border-radius: 10px;
	background-color: rgb(50 169 153);
	text-align: center;
	color: #fff;
	padding: 15px;
}

.wrapper h1{
	margin-top: 15px;
}
.wrapper p {
	margin-top: 20px;
}
.wrapper input {
	margin-top: 39px;
	background: transparent;
	border: 0;
	border-bottom: 2px solid #110813;
	color: aliceblue;
	font-size: 14px;
	padding: 5px 8px;
	margin-right: 15px;
	outline: none;
	transition: .3s;
}

.wrapper input:focus {
	border-bottom-color: rgb(71, 218, 255);
}

.wrapper button {
	background-color: #4c7ff7e6;
	color: #fff;
	border-radius: 5px;
	padding: 6px 15px;
	margin-left: 20px;
	cursor: pointer;
	transition: background-color transform 500ms ease;
}
.wrapper button:hover {
	transform: scale(1.1) translateY(-5px);
	background-color: rgb(76, 169, 202);
}

.error {
	color: tomato;
}
</style>
