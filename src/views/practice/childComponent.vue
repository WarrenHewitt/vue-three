<template>
    <div>
        prop: name = {{ name }}
        <div>
            <button @click="updateType">change type value</button>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent, toRefs } from 'vue'

/* 子组件 */
export default defineComponent({
    props: {
        name: {
            type: String,
            default: 'xxx'
        }
    },

    setup (props, context) {
        const { name } = toRefs(props)
        console.log(name.value)

        return {
            updateType: () => {
                context.emit('callback', Math.random())
            }
        }
    }
})
</script>

<style scoped>

</style>
