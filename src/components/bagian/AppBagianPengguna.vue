<template>
    <section class="pengguna mt-5">
        <div class="row">
            <div class="col-sm-6">
                <div class="card mb-3">
                    <div class="card-header text-white bg-info">
                        <h4 class="card-title"><i class="fas fa-sign-in-alt"></i> Login</h4>
                    </div>
                    <div class="card-body">
                        <app-formulir-input
                            v-model="namaPenggunaMasuk"
                            nama="masuk"
                            label="Nama Pengguna"
                            class="margin-bottom"
                        >
                        <template #aksi="">
                            <app-tombol
                                nama="masuk"
                                label="Masuk"
                                class="margin-left"
                                @klik="ketikaTombolMasukDiKlik"
                            />
                        </template>
                        </app-formulir-input>
                    </div>
                </div>
            </div>
            <div class="col-sm-6">
                <div class="card mb-3">
                    <div class="card-header text-white bg-info">
                        <h4 class="card-title"><i class="fas fa-user-plus"></i> Daftar Pengguna</h4>
                    </div>
                    <div class="card-body">
                        <app-formulir-input
                            v-model="namaPenggunaDaftar"
                            nama="daftar"
                            label="Nama Pengguna"
                            class="margin-bottom"
                        >
                        <template #aksi="">
                            <app-tombol
                                nama="daftar"
                                label="Daftar"
                                class="margin-left"
                                @klik="ketikaTombolDaftarDiKlik"
                            />
                        </template>
                        </app-formulir-input>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
export default {
    data() {
        return {
            namaPenggunaMasuk: 'fauzan',
            namaPenggunaDaftar: null
        }
    },
    methods: {
        ketikaTombolMasukDiKlik() {
            this.$store.dispatch('pengguna/masuk', {
                namaPengguna: this.namaPenggunaMasuk
            })
        },
        ketikaTombolDaftarDiKlik() {
            this.$store.dispatch('pengguna/daftar', {
                namaPengguna: this.namaPenggunaDaftar
            })
        },
    }
}
</script>