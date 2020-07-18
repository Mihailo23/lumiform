<template>
    <div>{{question}}</div>
</template>
<script>
export default {
    props: {
        question: {
            type: Object,
            default: () => {}
        }
    }
}
</script>