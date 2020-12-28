<template>
  <q-page class="flex flex-center" style="background-color: white">
    <div class="q-pa-md q-gutter-sm">
      <q-btn
      color="primary"
      icon-right="note" label="Profile"
        outline style="color: goldenrod;"
        to="/jackpot"
      >
      </q-btn>
      <q-btn
      color="negative"
      icon-right="assessment" label="Scratch Aplication"
        to="/scratch"
        push
      >
      </q-btn>
      <q-btn
      color="positive"
      icon-right="mail" label="Mail"
        unelevated
        to="*"
        flat rounded
      >
      </q-btn>
  
  </div>
  </q-page>
</template>

<script>

export default {
  name: 'PageIndex',
  data() {
    return {
      articles: []
    }
  },
}
</script>
