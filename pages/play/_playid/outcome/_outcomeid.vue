<template>
  <b-card class="container text-center mt-2 bg-light" >
    <h3>Fase {{ play.id }}</h3>
    <hr>
    <b-card-text :class="[outcome.correct == false ? 'text-danger' : 'text-success']"> {{ outcome.result }} </b-card-text>
    <div v-if="outcome.victory">
      <NuxtLink to="/">
        <b-button variant="outline-primary">Retornar ao início</b-button>
      </NuxtLink>
    </div>
    <div v-else-if="outcome.correct">
      <NuxtLink :to="`/play/${(play.id+1)}`">
        <b-button variant="outline-success">Prosseguir</b-button>
      </NuxtLink>
    </div>
    <div v-else>
      <NuxtLink to="/">
        <b-button variant="outline-danger">Sair</b-button>
      </NuxtLink>
    </div>
  </b-card>
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