<template>
	<div class="game-board">
		<Tile
			@tile-click="$emit('tile-click', tile.id)" 
			v-for="(tile, i) in tiles" 
			:key="i" 
			:tile="tile"
			:isDisabled="isDisabled"
		/>
	</div>
</template>

<script>
import Tile from "./Tile"

export default {
  name: 'Board',
  props: ['tiles', 'isDisabled'],
  components: {
    Tile
  },
}
</script>

<style scoped lang="scss">
	.game-board {
		display: grid;
		grid-template-columns: repeat(2, 150px);
		grid-template-rows: repeat(2, 150px);
		grid-gap: 5px;
		margin: 10px;
	}
</style>
