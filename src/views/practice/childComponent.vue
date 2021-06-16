<template>
    <div>
        <div>----------------</div>
        <h1>in child</h1>
        prop: name = {{ name }}
        <p>
            keyRef: {{ keyRef }}
        </p>
        <div>
            <button @click="updateType">change type value</button>
        </div>
        <div>----------------</div>
    </div>
</template>

<script lang="ts">
import { defineComponent, toRefs, inject } from 'vue'

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
        console.log('child prop: ', name.value)
        console.log('child inject: ', inject('key'))
        const keyRef = inject('keyRef')
        console.log('child inject: ', keyRef)

        return {
            keyRef,
            updateType: () => {
                context.emit('callback', Math.random())
            },
            parentCall: () => {
                console.log('parent call')
            }
        }
    }
})
</script>

<style scoped>

</style>
