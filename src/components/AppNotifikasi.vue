<template>
    <div
        v-if="$store.state.notifikasi.apakahTampil"
        class="notifikasi"
    >
      <i class="fas fa-info-circle"></i>  {{ $store.state.notifikasi.pesan }}
    </div>
</template>

<script>
    export default {
        name: 'AppNotifikasi',
    }
</script>

<style>
    .notifikasi {
        border: 1px solid #1a202c;
        background: rgb(57, 155, 122);
        position: fixed;
        color:#fff;
        right: 50px;
        top: 50px;
        width: 300px;
        height: auto;
        padding: 10px;
        border-radius: 5px;
    }
</style>
