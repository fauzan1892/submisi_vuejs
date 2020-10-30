<template>
    <div class="tampilan-kode">
        <div class="clearfix mt-3"></div>
        <button class="btn btn-danger btn-sm float-right" 
            v-if="idKode"
            @click="ketikaTombolHapusDiKlik"
        >
        Hapus
        </button>
        <div class="clearfix mb-2"></div>
        <data-code
            :code="kode"
            :lang="bahasaPemrograman"
            :is-highlighted="apakahHighlightMenyala"
        />
    </div>
</template>

<script>
import { validator } from '../utils'

export default {
    name: 'AppKode',
    props: {
        idKode: {
            type: String,
            default: null
        },
        kode: {
            default: '',
            required: true,
            validator
        },
        bahasaPemrograman: {
            default: '',
            required: true,
            validator
        },
        apakahHighlightMenyala: {
            type: Number,
            default: 1
        }
    },
    methods: {
        async ketikaTombolHapusDiKlik() {
            const hasilKonfirmasi = confirm('Apakah kamu yakin?')
            if (hasilKonfirmasi && this.idKode) {
                await this.$store.dispatch('kode/hapusKode', {
                    idPengguna: this.$store.state.pengguna.idPengguna,
                    idKode: this.idKode
                })
                await this.$emit('terhapus')
            }
        },
    }
}
</script>
