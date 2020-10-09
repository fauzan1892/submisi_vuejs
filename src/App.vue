<template>
    <main>
        <nav class="navbar navbar-expand-sm navbar-dark bg-info">
            <div class="container">
                <a class="navbar-brand" href=""><b><i class="fas fa-code"></i> Submisi Vue.js</b></a>
                <button v-if="$store.state.pengguna.idPengguna" class="navbar-toggler text-white d-lg-none" type="button"  @click="ketikaTombolKeluarDiKlik" data-toggle="collapse" data-target="#collapsibleNavId" aria-controls="collapsibleNavId"
                    aria-expanded="false" aria-label="Toggle navigation"><i class="fas fa-sign-out-alt"></i></button>
                <div class="collapse navbar-collapse" id="collapsibleNavId">
                    <ul class="navbar-nav ml-auto mt-2 mt-lg-0">
                        <li class="nav-item active" v-if="$store.state.pengguna.idPengguna">
                            <a class="nav-link" href="javascript:void(0)" @click="ketikaTombolKeluarDiKlik">
                                <i class="fas fa-sign-out-alt"></i>  Logout</a>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
        <div class="mt-5" v-if="$store.state.pengguna.idPengguna">
            <div class="container">
                <div class="card">
                    <div class="card-header text-white bg-info">
                        <h4 class="card-title"><i class="fas fa-file-alt"></i> Buat File</h4>
                    </div>
                    <div class="card-body">
                        <app-bagian-editor-opsi
                            :input-kode="dataKode.inputKode"
                            :bahasa-pemrograman-terpilih.sync="dataKode.bahasaPemrogramanTerpilih"
                            :twoslash-terpilih.sync="dataKode.twoslashTerpilih"
                            :nama-berkas.sync="dataKode.namaBerkas"
                            :highlight.sync="dataKode.highlight"
                            :hasil-highlight="hasilHighlight"
                            @tersimpan="dapatkanDaftarKode"
                            @reset="ketikaTombolResetDiKlik"
                        />
                    </div>
                </div>
                <br>
                <div class="card">
                    <div class="card-header text-white bg-info">
                        <h4 class="card-title"><i class="fas fa-code"></i> Buat Kode</h4>
                    </div>
                    <div class="card-body">
                        <app-bagian-editor-kode
                            :input-kode.sync="dataKode.inputKode"
                            :hasil-highlight="hasilHighlight"
                            :bahasa-pemrograman-terpilih="dataKode.bahasaPemrogramanTerpilih"
                        />
                    </div>
                </div>
                <br>
                <div class="card">
                    <div class="card-header text-white bg-info">
                        <h4 class="card-title"><i class="fas fa-file-alt"></i> Kode Saya </h4>
                    </div>
                    <div class="card-body">
                        <app-bagian-opsi-daftar-kode
                            :halaman.sync="filter.halaman"
                            :banyak-data.sync="filter.banyakData"
                            :urutkan-berdasarkan.sync="filter.urutkanBerdasarkan"
                            :urutkan.sync="filter.urutkan"
                            :apakah-highlight-menyala.sync="filter.apakahHighlightMenyala"
                        />
                        <div class="clearfix"></div>
                        <br>
                        <app-bagian-daftar-kode
                            :apakah-highlight-menyala="filter.apakahHighlightMenyala"
                            :dapatkan-daftar-kode="dapatkanDaftarKode"
                        />
                    </div>
                </div>
            </div>
            <br>
            <app-notifikasi />
            <app-proses />
            <div class="footer_after mt-3 pt-2 pb-2 bg-info text-white">
                <div class="container text-center">
                    <p> Submission for Vue.js Intermediate PENS 2020 Webinar</p>
                </div>
            </div>
        </div>
        <div v-else>
            <div class="container">
                <app-bagian-pengguna
                    @keluar="ketikaTombolResetDiKlik"
                />
                <br>
            </div>
            <div class="clearfix"></div>
            <br>
            <div class="footer pt-2 pb-2 bg-info text-white">
                <div class="container text-center">
                    <p> Submission for Vue.js Intermediate PENS 2020 Webinar</p>
                </div>
            </div>
        </div>
    </main>
</template>

<script>
    import { stringifyUrl } from 'query-string'
    import debounce from 'debounce-fn'
    import cleanDeep from 'clean-deep'

    import AppBagianPengguna from './components/bagian/AppBagianPengguna'
    import AppBagianEditorOpsi from './components/bagian/AppBagianEditorOpsi'
    import AppBagianEditorKode from './components/bagian/AppBagianEditorKode'
    import AppBagianOpsiDaftarKode from './components/bagian/AppBagianOpsiDaftarKode'
    import AppBagianDaftarKode from './components/bagian/AppBagianDaftarKode'

    import AppNotifikasi from './components/AppNotifikasi'
    import AppProses from './components/AppProses'

    import { dapatkanOpsi, kirimData, unduhKode } from './utils'
    import { URL_API, OPSI_STRINGIFY } from './constants'

    export default {
        name: 'App',
        components: {
            AppBagianPengguna,
            AppBagianEditorOpsi,
            AppBagianEditorKode,
            AppBagianOpsiDaftarKode,
            AppBagianDaftarKode,

            AppNotifikasi,
            AppProses
        },
        data() {
            return {
                dataKode: {
                    inputKode: null,
                    bahasaPemrogramanTerpilih: null,
                    namaBerkas: null,
                    highlight: null,
                    twoslashTerpilih: null,
                },
                filter: {
                    halaman: 1,
                    banyakData: 6,
                    urutkanBerdasarkan: 'createdAt',
                    urutkan: 'DESC',
                    apakahHighlightMenyala: 1
                },
                hasilHighlight: '',
                daftarBahasaPemrograman: [],
                show: false
            }
        },
        watch: {
            '$store.state.pengguna.idPengguna'(idPengguna) {
                if (idPengguna) {
                    this.dapatkanDaftarKode()
                }
            },
            dataKode: {
                handler: debounce(function(dataKode) {
                    this.hasilHighlight = ''
                    this.highlighter(dataKode.inputKode)
                }, { wait: 500 }), deep: true
            },
            filter: {
                handler: debounce(function() {
                    this.dapatkanDaftarKode()
                }, { wait: 500 }),
                deep: true
            }
        },
        async created() {
            await this.dapatkanDaftarBahasaPemrograman()
            await this.dapatkanDaftarKode()
        },
        methods: {
            ketikaTombolResetDiKlik() {
                this.dataKode = {
                    inputKode: null,
                    bahasaPemrogramanTerpilih: 'typescript',
                    namaBerkas: null,
                    highlight: null,
                    twoslashTerpilih: null,
                }
            },
            ketikaTombolKeluarDiKlik() {
                this.$store.dispatch('pengguna/keluar')
                this.$emit('keluar')
            },
            async dapatkanDaftarKode() {
                await this.$store.dispatch('kode/dapatkanSemuaKode', {
                    idPengguna: this.$store.state.pengguna.idPengguna,
                    filter: {
                        halaman: this.filter.halaman,
                        banyakData: this.filter.banyakData,
                        urutkanBerdasarkan: this.filter.urutkanBerdasarkan,
                        urutkan: this.filter.urutkan,
                        apakahHighlightMenyala: this.filter.apakahHighlightMenyala
                    }
                })
            },
            async ketikaTombolUnduhDiKlik() {
                try {
                    this.$store.dispatch('proses/tampilkanProses', null)
                    const objekUrl = {
                        url: URL_API,
                        query: {
                            lang: this.dataKode.bahasaPemrogramanTerpilih,
                            fileName: this.dataKode.namaBerkas,
                            highlight: this.dataKode.highlight,
                            twoslash: this.dataKode.twoslashTerpilih,
                            download: 1
                        }
                    }
                    const url = stringifyUrl(objekUrl, OPSI_STRINGIFY)
                    await unduhKode(url, {
                        code: this.dataKode.inputKode
                    })
                } catch (error) {
                    const dataNotifikasiGalat = {
                        apakahTampil: true,
                        pesan: error.message || 'Gagal mengunduh'
                    }
                    this.$store.dispatch('notifikasi/tampilkanNotifikasi', dataNotifikasiGalat)
                    console.log(error)
                } finally {
                    this.$store.dispatch('proses/hilangkanProses', null)
                }
            },
            async ketikaTombolSimpanDiKlik() {
                try {
                    const konten = cleanDeep({
                        kode: this.dataKode.inputKode,
                        bahasaPemrograman: this.dataKode.bahasaPemrogramanTerpilih,
                        highlight: this.dataKode.highlight,
                        namaBerkas: this.dataKode.namaBerkas,
                        twoslash: this.dataKode.twoslashTerpilih
                    })
                    await this.$store.dispatch('kode/simpanKode', {
                        idPengguna: this.$store.state.pengguna.idPengguna,
                        konten: konten
                    })
                    await this.dapatkanDaftarKode()
                } catch (error) {
                    const dataNotifikasiGalat = {
                        apakahTampil: true,
                        pesan: error.message || 'Gagal menyimpan'
                    }
                    this.$store.dispatch('notifikasi/tampilkanNotifikasi', dataNotifikasiGalat)
                    console.log(error)
                }
            },
            async dapatkanDaftarBahasaPemrograman() {
                try {
                    const respon = await dapatkanOpsi()
                    if (respon.success && !respon.error) {
                        this.daftarBahasaPemrograman = respon.data.languages
                    }
                } catch (error) {
                    const dataNotifikasiGalat = {
                        apakahTampil: true,
                        pesan: error.message
                    }
                    this.$store.dispatch('notifikasi/tampilkanNotifikasi', dataNotifikasiGalat)
                    console.log(error)
                }
            },
            async highlighter(inputKode, download) {
                try {
                    this.$store.dispatch('proses/tampilkanProses', null)
                    const objekUrl = {
                        url: URL_API,
                        query: {
                            lang: this.dataKode.bahasaPemrogramanTerpilih,
                            fileName: this.dataKode.namaBerkas,
                            highlight: this.dataKode.highlight,
                            twoslash: this.dataKode.twoslashTerpilih,
                            download
                        }
                    }
                        const url = stringifyUrl(objekUrl, OPSI_STRINGIFY)

                        const respon = await kirimData(url, {
                        code: inputKode
                    })

                    if (respon.success && !respon.error) {
                        this.hasilHighlight = respon.data
                    } else {
                        throw new Error(respon.message)
                    }
                } catch (error) {
                    const dataNotifikasiGalat = {
                        apakahTampil: true,
                        pesan: error.message || 'Bahasa Pemrograman dan Kode wajib diisi'
                    }
                    this.$store.dispatch('notifikasi/tampilkanNotifikasi', dataNotifikasiGalat)
                    console.log(error)
                } finally {
                    this.$store.dispatch('proses/hilangkanProses', null)
                }
            }
        }
    }
</script>
