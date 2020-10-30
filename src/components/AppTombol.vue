<template>
    <div :class="`${nama}`">
        <div  v-if="label === 'Reset'">
            <button class="btn btn-danger"
                :disabled="nonaktif"
                @click="ketikaTombolDiKlik"
            >
            {{ label }}
            </button>
        </div>
        <div v-else>
            <button class="btn btn-info"
                :disabled="nonaktif"
                @click="ketikaTombolDiKlik"
            >
            {{ label }}
            </button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'AppTombol',
    props: {
        nama: {
            type: String,
            required: true,
            default: ''
        },
        label: {
            type: String,
            required: true,
            default: ''
        },
        nonaktif: {
            type: Boolean,
            default: false
        }
    },
    methods: {
        ketikaTombolDiKlik(event) {
            this.$emit('klik', event)
        }
    }
}
</script>
