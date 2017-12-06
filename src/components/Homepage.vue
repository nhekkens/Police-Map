<template>
	<div class="containerFull">
		<router-view @route-data-loaded="changeTitle"></router-view>

		<gmap-map :center="center" :zoom="18" map-type-id="hybrid"	style="width: 100vw; height: 100vh">
			<gmap-marker
				:key="index"
				v-for="(m, index) in markers"
				:position="m.position"
				:clickable="true"
				:icon="m.icon"
				:draggable="false"
				@click="center=m.position"
   			></gmap-marker>
		</gmap-map>
	</div>
</template>

<script>
	import { EventBus } from './Parts/EventBus';

	export default {
		components: {},
		name: 'Homepage',
		mounted() {
			this.changeTitle(this.dataName);
		},
		props: {
			dataUrl: {
				type: String,
				default: 'unassigned'
		    }
		},
		data () {
			return {
				logging: true,
				dataName: 'Police Map',
				center: {
					lat: 51.520628,
					lng: -0.118575
				},
				markers: [
					{
						position: {
							lat: 51.520628,
							lng: -0.118575
						},
						icon: "./assets/images/office-marker.png"
					},
					{
						position: {
							lat: 11.0,
							lng: 11.0
						}
					}
				]
			}
		},
		methods: {
			changeTitle(Name) {
				if (this.logging) { console.log('Police Map •• changeTitle: ' + Name); }
				document.title = Name
			}	
		}
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style src="./../assets/scss/base.scss" lang="scss">

</style>
