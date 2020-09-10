<template>
    <div class="container">
        <div>
            <Logo />
            <h1 class="page-title">
                Deep-Food
            </h1>
			<div class="subtitle">
				Experience the power of AI with an Images link
			</div>
            <!-- <article class="message is-info">
                <div class="message-body">
                    Experience the power of AI with an Images link
                </div>
            </article> -->
            <div class="field has-addons has-addons-centered">
                <div class="control">
                    <input
                        class="input"
                        type="text"
                        placeholder="Deep search from an url..."
                        v-model="inputField"
                    />
                </div>
                <div class="control">
                    <a
                        class="button is-primary is-inverted is-outlined"
                        @click="predict"
                    >
                        Predict
                    </a>
                </div>
            </div>

            <div class="content" v-if="foodContents.length > 1">
                <div class="columns is-centered">
                    <div class="column is-7">
                        <div class="box">
                            <div class="card-content">
                                <div class="columns is-centered">
                                    <div class="column">
                                        <div class="columns">
                                            <div class="column">
                                                <span>Predicted concept</span
                                                >
                                            </div>
                                            <div class="column">
                                                <span class="tag-prob">Probability</span>
                                            </div>
                                        </div>
                                        <FoodContent
                                            v-for="foodContent in foodContents.slice(0,9)"
                                            :foodContent="foodContent"
                                            :key="foodContent.name"
                                        />
                                    </div>
                                    <div class="column has-text-centered">
                                        <FoodImage :imageUrl="imageUrl" />
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <section class="section">
                <div class="book">
                    <div class="book__form">
                        <!-- <ImageLinkForm
                            :rank="rank"
                            :onImageLinkSubmit="onImageLinkSubmit"
                            :onImageLinkChange="onImageLinkChange"
                            :foodContents="foodContents"
                        /> -->
                    </div>
                </div>
            </section>

            <ColorModePicker />
        </div>
    </div>
</template>

<script>
import Clarifai from "clarifai";
import ImageLinkForm from "@/components/ImageLinkForm";
import FoodImage from "@/components/FoodImage";
import foodContent from "@/components/FoodContent";
import ColorModePicker from "@/components/ColorModePicker";

export default {
    name: "Food",
    components: {
        ImageLinkForm,
        FoodImage,
        foodContent,
        ColorModePicker
    },
    data() {
        return {
			imageUrl: '',
			inputField: '',
			foodContents: [],
			rank: 0
        };
	},
	mounted() {
		window.console.log("YEEEEEEET")
		console.log(this.$config.baseURL)
		console.log(this.$config.test)
	},
    methods: {
        displayFoodContent(data) {
            const foodItems = data.outputs[0].data.concepts;
            return foodItems.map(foodItem => {
                const name = foodItem.name;
                const val = foodItem.value;
                return {
                    name,
                    val
                };
            });
        },
        predict() {
            const app = new Clarifai.App({
                apiKey: this.$config.apiKey || {}
            });
            this.imageUrl = this.inputField;

            //Make request to the clarifai face detection model api endpoint
            app.models
                .predict(Clarifai.FOOD_MODEL, this.imageUrl)
                .then(response => {
                    // listFood(displayFoodContent(response));
                    // incrementRank();
                    this.foodContents = this.displayFoodContent(response);
                })
                .catch(err => console.log(err));
        }
    }
};
</script>
