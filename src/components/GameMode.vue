<template>
    <div @change="$emit('mode', $event.target.value)">
        <h2>Режим игры:</h2>
        <Mode 
            v-for="(mode, i) in gameModes" :key="i" 
            :mode="mode"
        />
    </div>
</template>

<script>
import Mode from './Mode'

export default {
    name: 'GameMode',
    components: {
        Mode
    },
    props: ['gameModes']
}


</script>