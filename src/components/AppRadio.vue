<template>
    <div :class="`opsi-formulir-${nama}`" class="opsi-formulir">
         <div class="form-group">
            <label :for="nama">{{ label }} </label>
            <div>
                    <div
                        v-for="(pilihan, i) in daftarPilihan"
                        :key="i"
                    >
                    <input
                        v-model.number="nilai"
                        type="radio"
                        :value="pilihan.nilai"
                        name="highlight-menyala"
                        :id="`${nama}-${i}`"
                    > 
                    <label :for="`${nama}-${i}`" class="pl-2">
                        {{ pilihan.teks }}
                    </label>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { formulir } from '../mixins'

export default {
    name: 'AppRadio',
    mixins: [formulir],
    props: {
        daftarPilihan: {
            type: Array,
            required: true,
            default() {
                return []
            }
        }
    },
    data() {
        return {
            nilai: 1
        }
    },
    watch: {
        nilai: {
            handler(nilai) {
                this.$emit('input', nilai)
            },
            immediate: true
        }
    }
}
</script>
