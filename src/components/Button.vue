<template>
  <button type="button" 
   :class="{ operation, advanced, extended, equals }" 
    v-html="label"
   @click="$emit('onCalc', label)"></button>
</template>

<script>
export default {
    props: {
        label: {},
        operation: { type: Boolean },
        advanced: { type: Boolean },
        extended: { type: Boolean },
        equals: { type: Boolean }
    }
}
</script>