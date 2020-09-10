<template>
    <h2 class="heading-secondary">#{{ rankCount }}</h2>
</template>
<script>
export default {
    name: 'Rank',
    props: {
        rankCount: {
            type: Number,
            required: true
        }
    }
};
</script>
<style></style>
