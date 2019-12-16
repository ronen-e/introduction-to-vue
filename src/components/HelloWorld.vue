<template>
	<main>
		<section>
			<header>IP Generator</header>
			<article>
				<p>
					<input tabindex="1" v-model="query" placeholder="IP or Domain name" type="text" autocomplete="off" />
				</p>
				<button tabindex="1" :disabled="loading" @click="getData()">Get GEO data</button>
			</article>
			<article v-if="result">
				<p>Here is your GEO data</p>
				<pre>{{result | json}}</pre>
			</article>
		</section>
	</main>
</template>

<script>
	export default {
		name: "HelloWorld",

		data() {
			return {
				loading: false,
				query: '',
				result: ''
			};
		},
		methods: {
			getData() {
				if (this.loading) return;

				this.loading = true;
				fetch("http://ip-api.com/json/" + this.query)
					.then(res => res.json())
					.then(this.update)
					.catch(this.onError)
					.finally(() => this.loading = false)
			},
			update(data) {
				this.result = data;
			},
			onError(e) {
				alert(e);
			}
		},

		filters: {
			json(value) {
				if (!value) return ''
				return JSON.stringify(value, null, 2);
			}
		}		
	};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	section {
		padding: 0 80px;
	}
	article {
		padding: 10px;
		margin: 20px;
		border: 1px solid gainsboro;
	}

	button {
		background-color: rgb(83, 170, 83);
		padding: 10px;
		border-radius: 5px;
		color: white;
		cursor: pointer;
	}
	button[disabled] {
		background-color: red;
		cursor: not-allowed;
	}
</style>
