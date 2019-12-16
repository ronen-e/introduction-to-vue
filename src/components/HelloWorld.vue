<template>
	<main>
		<v-container>
			<v-layout wrap>
				<v-flex xs12 text-center>
					<header>IP Generator</header>
				</v-flex>

				<v-flex xs12 md6>
					<article class="pa-2 ma-3">
						<v-text-field
							v-model="query"
							outlined
							prepend-icon="mdi-map-marker"
							rounded
							label="Query"
							clearable
							autofocus
							tabindex="1"
							placeholder="IP or Domain name"
							:autocomplete="false"
							@keyup.enter="getData()"
							:loading="loading"
						></v-text-field>
						<v-btn block rounded color="primary" tabindex="1" :loading="loading" @click="getData()">Get GEO data</v-btn>
					</article>
					
				</v-flex>

				<v-flex v-if="result" xs12 md6>
					<article class="pa-2 ma-3">
						<p>Here is your GEO data</p>
						<pre>{{result | json}}</pre>
					</article>
				</v-flex>
			</v-layout>
		</v-container>
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

	article {
		margin: 20px;
		border: 1px solid gainsboro;
		overflow: auto;
	}
</style>
