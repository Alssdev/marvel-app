<template>
  <div></div>
</template>

<script>
    export default {
        data() {
            return {
                perPage: 16,
                rangeBefore: 1,
                rangeAfter: 1,
            }
        },
        
        props: {
            total: {
                type: Number,
                required: true
            },
            current: {
                type: Number,
            }
        }
    }
</script>
