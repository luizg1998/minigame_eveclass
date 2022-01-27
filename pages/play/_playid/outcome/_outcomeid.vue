<template>
  <div>
    <p> {{ outcome.result }} </p>
    <div v-if="outcome.victory">
      <p>VICTORY</p>
      <NuxtLink to="/`">Sair</NuxtLink>
    </div>
    <div v-else-if="outcome.correct">
      <p>YESSSS</p>
      <NuxtLink :to="`/play/${(play.id+1)}`">Prosseguir</NuxtLink>
    </div>
    <div v-else>
      <p>Oh no</p>
      <NuxtLink to="/">Sair</NuxtLink>
    </div>
  </div>
</template>

<script>
export default {
  props: ['play'],
  computed: {
      outcome() {
        return this.play.outcomes.find(o => o.id == this.$route.params.outcomeid)
      }
    }
}
</script>