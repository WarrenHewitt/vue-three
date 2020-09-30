<template>
    <div>
        <section>
            {{ type }}
            <div>

            </div>
        </section>
        <section>
            <slot name="cusSlot"></slot>
        </section>
        <ChildComponent name="23333" @callback="updateType"/>
    </div>
</template>

<script lang="ts">
import { defineComponent, onBeforeMount, onMounted, reactive, toRefs } from 'vue'
import ChildComponent from '@/views/practice/childComponent.vue'

export default defineComponent({
    components: {
        ChildComponent
    },

    setup (props, context) {
        console.log('setup')

        console.log(props)

        console.log(context.slots)

        const state = reactive({ type: 'keyboard' })

        const updateType = (v: string) => {
            console.log(typeof v)
            state.type = v
        }

        onBeforeMount(() => {
            console.log('beforeMount')
        })

        onMounted(() => {
            console.log('Mounted')
        })

        return {
            ...toRefs(state),
            updateType
        }
    }

})
</script>

<style scoped>

</style>
