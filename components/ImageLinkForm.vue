<template>
    <div>
        <form @submit.prevent="onImageLinkSubmit" class="form">
            <div class="u-margin-bottom-medium">
                <h2 class="heading-primary">
                	Your current image count is....
                </h2>
            </div>
            <div class="t-center u-margin-bottom-medium">
                <Rank :rankCount="rank" />
            </div>
        </form>
    </div>
</template>
<script>
import Rank from "./Rank";

export default {
    name: "ImageLinkForm",
    components: {
        Rank
    },
    props: {
        rank: {
            type: Number,
            required: true
        },
        onImageLinkSubmit: {
            type: Function
        },
        onImageLinkChange: {
            type: Function
        }
    }
};
</script>