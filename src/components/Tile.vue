<template>
  <button
    :style="style" 
    @click="$emit('tile-click', tile.id)" 
    :id="tile.id"
    :class="{active: tile.isActive}"
    :disabled="isDisabled"
  />
</template>

<script>
export default {
  name: 'Tile',
  props: ['tile', 'isDisabled'],
  computed: {
      style () {
        // let color = this.tile.id * 90;
        // return `background-color: hsl(${color}, 100%, 30%)`;
        return `background-color: ${this.tile.color}`;
      }
    }
}
</script>

<style scoped lang="scss">
  
  button {
    outline: none;
    border: 1px solid black;
    opacity: .5;
  }

  button:hover {
    border: 5px solid  black;
  }

  button:active {
    transform: scale(0.94);
    opacity: 1;
  }

  .active {
		opacity: 1;
	}

</style>
