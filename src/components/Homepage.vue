<template>
	<div class="containerFull">
		<router-view @route-data-loaded="changeTitle"></router-view>

		
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
				dataName: 'Leaky - Home'
			}
		},
		methods: {
			changeTitle(Name) {
				if (this.logging) { console.log('Leaky •• changeTitle: ' + Name); }
				document.title = Name
			}	
		}
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style src="./../assets/scss/base.scss" lang="scss">

</style>
