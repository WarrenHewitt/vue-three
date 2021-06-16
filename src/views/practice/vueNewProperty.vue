<template>
    <div>
        <section>
            {{ type }}
            <div>
                <p>a： {{ a }}</p>
                <p>add： {{ add }}</p>
                <button @click="changeA">修改a</button>
            </div>
            <div>
                <p>addGetSet： {{ addGetSet }}</p>
                <button @click="computedGet">computed get</button>
                <button @click="computedSet">computed set</button>
            </div>
            <div>
                <h1>Watch</h1>
                <p> w1: {{ w1 }} </p>
                <div>
                    <button @click="watchData">update watch w</button>
                </div>
                <p> w2: {{ w2 }} </p>
                <div>
                    <button @click="watchData1">update watch w2</button>
                    <button @click="handleStop">stop</button>
                </div>
                <p> m1: {{ m1 }} </p>
                <div>
                    <button @click="watchM1">update watch m1</button>
                </div>
                <p> m2: {{ m2 }} </p>
                <div>
                    <button @click="watchM2">update watch m2</button>
                </div>
            </div>
        </section>
        <section>
            <slot name="cusSlot"></slot>
        </section>
        <section style="margin: 10px">
            <input ref="inputRef" type="text">

            <p>
                <button @click="getChildMethod">call child method</button>
            </p>
        </section>
        <ChildComponent name="childProp" @callback="updateType" ref="childRef"/>
    </div>
</template>

<script lang="ts">
import { defineComponent, onBeforeMount, onMounted, reactive, toRefs, ref, isRef, computed, watch, provide } from 'vue'
import ChildComponent from '@/views/practice/childComponent.vue'

export default defineComponent({
    components: {
        ChildComponent
    },

    setup (props, context) {
        console.log(props)

        console.log(context.slots)

        const a = ref(1)
        const a1 = ref(2)
        const a2 = 'a2'
        const c = ref(1001)

        const state = reactive({ type: 'keyboard', a1 })

        console.log('a1:', state.a1)
        console.log('is ref:', isRef(a1), isRef(a2))

        const updateType = (v: string) => {
            console.log(typeof v)
            state.type = v
        }

        onBeforeMount(() => {
            console.log('beforeMount')
        })

        const inputRef = ref()
        const childRef = ref()

        onMounted(() => {
            console.log('Mounted')
            inputRef.value.value = 'red'
            console.log('inputRef:', inputRef)
        })

        const add = computed(() => (a.value + 0.1))

        const addGetSet = computed({
            get: () => c.value,
            set: (v) => { c.value = v }
        })

        const stateW = reactive({ w: 1, w1: 'hew', w2: '2' })
        const wr = ref(23)

        watch(
            /* 监听 reactive 要用函数返回的形式 */
            () => { console.log('watch auto execute'); return stateW.w },
            (w, oldW) => {
                stateW.w1 = Math.random().toString()
                console.log('w:', w, oldW)
            }
        )
        let n = 1
        const asyncFn = () => {
            return setTimeout(() => {
                console.log(n)
                n++
            }, 5000)
        }

        const stop = watch(
            /* 监听 ref 直接传递即可 */
            wr,
            (wr, oldWr, onCleanup) => {
                stateW.w2 = Math.random().toString()
                console.log('wr:', wr, oldWr)
                const idStr = asyncFn()
                onCleanup(() => {
                    clearTimeout(idStr)
                })
            }
        )

        const stateMultiple = reactive({ m1: 1, m2: 2 })

        provide('key', 'parent provide value')
        provide('keyRef', a)
        watch(
            /* 监听 多个 */
            [() => stateMultiple.m1, () => stateMultiple.m2],
            ([m1, m2], [m1o, m2o]) => {
                console.log('m1', m1, m1o)
                console.log('m2', m2, m2o)
            }
        )

        return {
            inputRef,
            childRef,
            a,
            add,
            addGetSet,
            wr,
            ...toRefs(state),
            ...toRefs(stateW),
            ...toRefs(stateMultiple),
            updateType,
            changeA: () => {
                a.value = a.value + 1236
            },
            computedGet: () => {
                console.log(addGetSet.value)
            },
            computedSet: () => {
                addGetSet.value = 2333 + Math.random()
            },
            watchData: () => {
                stateW.w = Math.random()
            },
            watchData1: () => {
                wr.value = Math.random()
            },
            handleStop: () => {
                stop()
            },
            watchM1: () => {
                stateMultiple.m1 = Math.random()
            },
            watchM2: () => {
                stateMultiple.m2 = Math.random()
            },
            getChildMethod: () => {
                childRef.value.parentCall()
            }
        }
    }
})
</script>

<style scoped>

</style>
