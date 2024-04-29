<!-- What you see -->
<template>
    <div style="background-color: lightblue; margin-bottom: 20px;">
        <h2>
            {{ studentname }}
        </h2>
        <h3 :style="getColor()">
            Counter: {{ result }}
        </h3>

        <p>
            Initial value: <span v-once> {{ result }}</span>
        </p>

        <!-- // :disabled="result >= 3" -->
        <button @click="plusOne()" >
            Add one
        </button>

        <button @click="minusOne()" v-if="result > 0">
            Minus one
        </button>
    </div>
</template>

<!-- Settings -->
<script>
    export default {
        name: "CounterComponent",
        props: ["studentname"],
        data() {
            return {
                name: "Collin",
                result: 0
            }
        },
        methods: {
            getColor() {
                if (this.result < 8) {
                    return "color: red";
                } else if (this.result < 10) {
                    return "color: orange";
                } else {
                    return "color: green";
                }
            },
            plusOne() {
                this.result++;
            },
            minusOne() {
                this.result--;
            }
        }
    }
</script>