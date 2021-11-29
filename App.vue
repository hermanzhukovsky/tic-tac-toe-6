<template>
	<hello-world></hello-world>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import { mapGetters, mapMutations } from 'vuex';
export default {
	name: "App",
	components: {
		HelloWorld,
	},
	computed: mapGetters(['mode', 'isGameOver', 'emptySquaresCount']),
	methods: {
		...mapMutations([
			'updateSquares', 
			'calculateWinner', 
			'toggleTurn',
			'updateCount',
			'updateTurn',
			'resetGame',
			
		]),
		onStorageUpdate(e) {
			if (e.key === 'history') {
				this.updateSquares();
			
				if (this.isGameOver) {
					this.resetGame();
					return;
				}
				
				this.calculateWinner();
				
			}
			if (e.key === 'count') {
				this.updateCount();
			}
			if (e.key === 'multiplayerTurn') {
				this.updateTurn();
				this.toggleTurn();
			}
		}
	},
	created() {
		if (this.mode === 'multiplayer') {
			this.$store.dispatch('initMultiplayerMode')
		}
	},
	mounted() {
		window.addEventListener('storage', this.onStorageUpdate)
	}
};
</script>

<style lang="scss">
</style>
