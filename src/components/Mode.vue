<template>
    <label>
        <input type="radio" name="mode" :value="mode.value" :checked="mode.checked">{{mode.text}}<br>
    </label>
</template>

<script>

export default {
    name: 'Mode',
    props: ['mode']
}


</script>