<template>
    <div>
        平级1
    </div>
    <div>
        平级1
    </div>
    <div>
        {{ count }}
    </div>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component'

// Define the component in class-style 130130

@Options({
    components: {
    }
})

export default class Test extends Vue {
    // Class properties will be component data
    count = 0

    // Methods will be component methods
    increment () {
        this.count++
    }

    decrement () {
        this.count--
    }
}
</script>

<style scoped>

</style>
