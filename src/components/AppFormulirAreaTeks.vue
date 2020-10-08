<template>
    <div :class="`opsi-formulir-${nama}`">
        <label :for="nama">
            {{ label }} 
        </label>
        <br/>
        <textarea class="form-control"
            :value="value"
            :id="nama"
            :class="nama"
            @input="ketikaNilaiBerubah" rows="7"
        ></textarea>
        <br>
    </div>
</template>

<script>
import { formulir } from '../mixins'
export default {
    name: 'AppFormulirAreaTeks',
    mixins: [formulir],
    methods: {
        ketikaNilaiBerubah(event) {
            this.$emit('input', event.target.value)
        }
    }
}
</script>
