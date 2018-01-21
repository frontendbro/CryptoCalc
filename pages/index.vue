<template>
	<div id="app">
		<section class="container">
			<Header />
			<App />
			<div class="wrapper">
				<form action="">
					<input type="text" v-model="amount">
					<select v-model="choosenCode" >
						<option v-for="(name, code) in currencies" :value="code" :key="code">{{name}}</option>
					</select>
					<button @click.prevent="lookup">смотреть</button>
				</form>
				<ul v-if="rates != null">
					<li v-for="(name, code) in currencies" :key="code">{{amount}} {{currencies[choosenCode]}} = {{convert(code)}} {{currencies[code]}}</li>
				</ul>
			</div>

		</section>
	</div>
</template>

<script>
import Header from '~/components/Header.vue';
import App from '~/components/App.vue';

export default {
	components: {
		Header, App
	},
	data(){
		return {
			currencies:{
				BTC: 'Bitkoin',
				USD: 'Dollar',
				EUR: 'Euro',
				ETH: 'Ethereum',
				LTC: 'Litecoin'
			},
			choosenCode: null,
			amount: 0,
			rates: null
		}
	},
	methods:{
		lookup(){
			const params = {fsym: this.choosenCode, tsyms: 'BTC,USD,EUR,ETH,LTC'};
			this.$http.get('https://min-api.cryptocompare.com/data/price', {params}).then((Response)=>{
				this.rates = Response.body;
			});
			// const codes = Object.keys(this.currencies).join(',');
			// fetch(`https://min-api.cryptocompare.com/data/price?fsym=${this.choosenCode}&tsyms=${codes}`).then((Response)=>{
			// 	console.log(Response);
			// 	return Response.json().then((Data)=>{this.rates = Data});
			// });
		},
		convert(code){
			return this.rates[code] * this.amount;
		}

	}
}
</script>

<style>
.container {
	display: flex;
	flex-direction: column;
}
.wrapper{
	flex-grow: 1;
}
</style>
