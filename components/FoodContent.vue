<template>
    <div>
		<div class="columns">
			<div class="column">
				<p>{{ foodContent.name }}</p>
			</div>
			<div class="column has-text-weight-bold">
                <p>{{ foodContent.val }}</p>
			</div>
		</div>
    </div>
</template>
<script>
export default {
    props: {
        foodContent: {
            type: Object
        }
    }
};
</script>