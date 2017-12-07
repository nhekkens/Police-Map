<template>
	<div class="controlPanel">
		<div class="form-group">
			<label class="col-form-label" for="lat"></label>
			<input class="form-control" v-model="args.lat" type="number" name="lat">
		</div>
		<div class="form-group">
			<label class="col-form-label" for="lng"></label>
			<input class="form-control" v-model="args.lng" type="number" name="lng">
		</div>
		<select class="form-control" v-model="args.crimeType">
	        <option v-for="crimeType in crimeTypes" :value="crimeType.url">{{crimeType.name}}</option>
	    </select>
	    <button class="btn btn-primary" @click="filterMap">Filter</button>
	</div>
</template>

<script>
	import { EventBus } from './EventBus';

	export default {
		name: 'controlPanel',
		mounted() {
			this.calcDate();
			this.loadCategories();
		},
		data () {
			return {
				logging: false,
				crimeTypes: [],
				args: {
					crimeType: 'all-crime',
					date: '',
					lat: 51.520628,
					lng: -0.118575
				}
			}
		},
		methods: {
			calcDate() {
				// Im doing this becasue 2017-11 doesnt return crimes ( it should only show data for the previous month. )
				// let date = new Date();
				// this.args.date = date.getFullYear() + '-' + ( date.getMonth());
				this.args.date = '2017-09';
				if (this.logging) { console.log('Control Panel •• Date: ' + this.args.date); }
				EventBus.$emit('filterMap', this.args);
			},
			loadCategories() {
				if (this.logging) { console.log('Control Panel •• loadCategories()'); }
				this.$http.get( 'https://data.police.uk/api/crime-categories?date=' + this.args.date ).then(function(response) {
					if (this.logging) { console.log(response); }
		            this.crimeTypes = response.body;
	            }, function(response) {
	                if (this.logging) { console.log(response); }
	            })
			},
			filterMap() {
				if (this.logging) { console.log('Control Panel •• filterMap()'); }
				EventBus.$emit('filterMap', this.args);
			}
		}
	}
</script>
