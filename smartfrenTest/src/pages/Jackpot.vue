<template>
  <div class="q-pa-md row items-start q-gutter-md">
    <q-card class="my-card" flat bordered>
      <q-img
        src="~assets/2001669145.jpg"
      />

      <q-card-section>
        <div class="text-overline text-orange-9">Biodata Singkat</div>
        <div class="text-h5 q-mt-sm q-mb-xs">Ade Rajab Ismail</div>
        <div class="text-caption text-grey">
          Ttl: Jakarta, 06 Desember 1994
        </div>
        <div class="text-caption text-grey">
          Alamat: Jl. Kapuk Raya No.3
        </div>
      </q-card-section>

    </q-card>
  </div>
</template>

<script>
export default {
  name: 'Jackpot',
  data () {
    return {
      expanded: false,
      lorem: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.'
    }
  }
}
</script>

<style lang="sass" scoped>
.my-card
  width: 100%
  max-width: 350px
</style>