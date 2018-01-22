<template>
	<div id="app">
		<section class="container">
			<Header />
			<div class="wrapper">
				<div class="calc-wrap">

					<form class="calc-form">
						<div class="select-box">

							<div class="select-box__item">
								<p class="select-h">Currency</p>
								<label class="select">

									<select v-model="choosenCode" id="sel">
										<option v-for="(name, code) in currencies" :value="code" :key="code">{{name}}</option>
									</select>
								</label>
							</div>
							<div class="select-box__item">
								<p class="select-h">Number of coins</p>

								<input type="text" v-model="amount" class="calc-form__input" placeholder="0">
							</div>
						</div>
						<p class="select-h">Relative to</p>
						<label class="select">

							<select v-model="choosenCurr">
								<option v-for="(name, code) in currencies" :value="code" :key="code">{{name}}</option>
							</select>
						</label>
						<button @click.prevent="lookup" class="calc-form__button" v-if=" amount <= 10000000000">Result</button>
					</form>
					<ul v-if="rates != null" class="list-currency">
						<li class="item-currency">{{amount}} {{currencies[choosenCode]}} = {{convert(choosenCurr)}} {{currencies[choosenCurr]}}</li>
					</ul>
				</div>
				<div class="graph-wrap">
					<App />
				</div>
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
			choosenCode: 'USD',
			choosenCurr: 'BTC',
			amount: null,
			rates: null,
		}
	},
	methods:{
		lookup(){
			const params = {fsym: this.choosenCode, tsyms: this.choosenCurr};
			this.$http.get('https://min-api.cryptocompare.com/data/price', {params}).then((Response)=>{
				this.rates = Response.body;
			});
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
	display: inline-flex;
	align-self: flex-start;
	flex-grow: 1;
	margin: 30px 90px;
	padding: 16px 32px;
	background-color: #fff;
	box-shadow: 0 2px 4px 0 rgba(0,0,0,0.15), 0 0 0 1px rgba(0,0,0,0.06);
	border-radius: 2px;
}
.calc-wrap{
	display: flex;
	flex-direction: column;
	margin-right: 30px;
}
.graph-wrap{
	display: flex;
}
.calc-form{
	width: 300px;
	display: flex;
	flex-direction: column;
	box-shadow: 0 2px 4px 0 rgba(0,0,0,0.15), 0 0 0 1px rgba(0,0,0,0.06);
	padding: 16px;
	margin-bottom: 16px;
	border-radius: 2px;
	background-color: #f6f6f6;
}
.calc-form__input{
	padding: 4px;
	outline: none;
	border-radius: 2px;
	border: 1px solid #d3d3d3;
	margin-bottom: 10px;
	font-size: 14px;
}
.calc-form__button{
	background-color: #fff;
	border-radius: 2px;
	border: 1px solid #d3d3d3;
	padding: 4px 10px;
	margin-top: 10px;
	align-self: flex-start;
	font-size: 12px;
	text-transform: uppercase;
	opacity: 0.7;
	outline: none;
	color: #000000;
	font-weight: 500;
	cursor: pointer;
}
.calc-form__button:hover{
	background-color: #4A90E2;
	color: #fff;
	border-color: transparent;
}
.list-currency{
	list-style-type: none;
	padding: 0;
}
.item-currency{
	border-bottom: 1px solid #D3D3D3;
	padding: 4px 0;
}

/*select*/
.select-box{
	display: flex;
	justify-content: space-between;
}
.select-box__item{
	display: flex;
	flex-direction: column;
}
.select-box__item:first-child{
	flex-grow: 1;
	margin-right: 10px;
}
.select{
	display: inline-block;
	position: relative;
	width: 100%;
	height: 26px;
	border-radius: 2px;
	border: 1px solid #d3d3d3;
	overflow: hidden;
	background-color: #fff;
}
.select-h{
	font-size: 12px;
	margin-bottom: 8px;
	opacity: 0.6;
	font-weight: 500;
	color: #000
}
.select:after{
	content: '';
	display: inline-block;
	position: absolute;
	top: 0;
	right: 0;
	height: 100%;
	width: 40px;
	background: #fff;
	pointer-events: none;
	z-index: 9;
}
.select:before{
	content: '';
	display: inline-block;
	position: absolute;
	width: 8px;
	height: 4px;
	background: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAgAAAAECAYAAACzzX7wAAAAMklEQVQIW2NkYGAwYWBgMGLADs4xQsWxKTrHwMBwBqYApA5ZEVgSJIisAKYIRIMlQQAAdf0GOebEttYAAAAASUVORK5CYII=") no-repeat center;
	top: 50%;
	margin-top: -2px;
	right: 7px;
	z-index: 10;
}
.select select{
	position: absolute;
	top: 0;
	right: 0;
	left: 0;
	bottom: 0;
	width: 100%;
	height: 100%;
	-moz-appearance: none;
	-webkit-appearance: none;
	border: 0;
	box-shadow: none;
	margin: 0;
	padding-left: 7px;
	outline: none;
	cursor: pointer;
	background: transparent;
	font-size: 14px;
}
/*select - end*/
</style>
