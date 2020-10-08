<template>
    <div :class="`opsi-formulir-${nama}`">
        <div class="form-group row">
            <label class="col-sm-4" :for="nama">{{ label }} </label>
            <div class="col-sm-8">
                <input
                    :value="value"
                    :type="tipe"
                    :id="nama"
                    class="form-control"
                    @input="ketikaNilaiBerubah"
                >
                <div class="clearfix mt-2"></div>
                <slot name="aksi"/>
            </div>
        </div>
    </div>

</template>

<script>
import { formulir } from '../mixins'
export default {
    name: 'AppFormulirInput',
    mixins: [formulir],
    props: {
        tipe: {
        type: String,
        default: 'text'
        }
    },
    methods: {
        ketikaNilaiBerubah(event) {
            if (this.tipe === 'number') {
                this.$emit('input', parseInt(event.target.value))
            } else {
                this.$emit('input', event.target.value)
            }
        }
    }
}
</script>
