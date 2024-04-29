<!-- What you see -->
<template>
    <div>
        <h2>
            Counter: {{ result }}
        </h2>

        <button @click="plusOne()">
            Add one
        </button>

        <button>
            Minus one
        </button>
    </div>
</template>

<!-- Settings -->
<script>
    export default {
        name: "CounterComponent",
        data() {
            return {
                result: 0
            }
        },
        methods: {
            plusOne() {
                this.result++;
            }
        }
    }
</script>