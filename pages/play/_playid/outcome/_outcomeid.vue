<template>
  <b-card class="container text-center mt-2 bg-light" >
    <h3>Fase {{ play.id }}</h3>
    <hr>
    <b-alert show :variant="outcome.correct == false ? 'danger' : 'success'">
      <b-icon v-if="outcome.victory" icon="trophy-fill"></b-icon>
      <b-icon v-else-if="outcome.correct" icon="emoji-laughing-fill"></b-icon>
      <b-icon v-else icon="emoji-frown-fill"></b-icon>
      {{ outcome.result }}
    </b-alert>
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
      // Baseado no id da url, esta função busca no array outcomes (dentro do play atual que foi passado como prop), o objeto outcome respectivo ao id
      return this.play.outcomes.find(o => o.id == this.$route.params.outcomeid)
    }
  }
}
</script>