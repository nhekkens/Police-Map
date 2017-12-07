<template>
	<div class="containerFull">
		<gmap-map :center="center" :zoom="15" map-type-id="hybrid"	style="width: 100vw; height: 100vh">
			<!-- Office marker -->
			<gmap-marker
				:key="index"
				v-for="(m, index) in office"
				:position="m.position"
				:clickable="true"
				:icon="m.icon"
				:draggable="false"
				@click="center=m.position"
   			></gmap-marker>
			
			<!-- Crime Markers -->
   			<gmap-marker
				:key="index"
				v-for="(crime, index) in crimes"
				:position="handleCoords(crime.location)"
				:clickable="true"
				:draggable="false"
				@click="center=handleCoords(crime.location)"
				@mouseover="infoWindowText = crime.location.street.name"
        		@mouseout="infoWindowText = null"
			></gmap-marker>

			<div slot="visible">
				<div class="infoWindow">
					{{infoWindowText}}
				</div>
			</div>
		</gmap-map>

		<control-panel></control-panel>
	</div>
</template>

<script>
	import { EventBus } from './Parts/EventBus';
	import controlPanel from './Parts/controlPanel';

	export default {
		components: {
			'control-panel': controlPanel
		},
		name: 'Homepage',
		mounted() {
			EventBus.$on('filterMap', (args) => {
				if (this.logging) { console.log('Event Bus •• filterMap received', args); }
				this.args = args;
				this.center.lat = parseFloat(args.lat);
				this.center.lng = parseFloat(args.lng);
				this.loadCrimes();
			});
			this.loadCrimes();
		},
		data () {
			return {
				logging: false,
				dataName: 'Police Map',
				infoWindowText: null,
				center: {
					lat: 51.520628,
					lng: -0.118575
				},
				args: {
					lat: 51.520628,
					lng: -0.118575,
					crimeType: 'all-crime',
					date: '2017-09'
				},
				office: [
					{
						position: {
							lat: 51.520628,
							lng: -0.118575
						},
						icon: "data:image/png;base64,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"
					}
				],
				crimes: []
			}
		},
		methods: {
			handleCoords(coords) {
				let newCoords = {
					lat: parseFloat(coords.latitude),
					lng: parseFloat(coords.longitude)
				}

				return newCoords
			},
			loadCrimes() {
				this.$http.get( 'https://data.police.uk/api/crimes-street/' + this.args.crimeType + '?lat=' + this.args.lat + '&lng=' + this.args.lng + '&date=' + this.args.date ).then(function(response) {
					if (this.logging) { console.log(response); }
		            this.crimes = response.body;
	            }, function(response) {
	                if (this.logging) { console.log(response); }
	            })
			},
		}
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style src="./../assets/scss/base.scss" lang="scss">

</style>
