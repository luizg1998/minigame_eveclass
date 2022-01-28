<template>
  <div>
    <!-- Elemento child, outcome -->
    <nuxt-child :play="play" />
    <!-- Fiz uma condição baseada no caminho, para não mostrar essa parte quando estiver no path do outcome -->
    <b-card v-if="this.$route.matched.length == 1" class="container text-center mt-2 bg-light" >
      <h3>Fase {{ play.id }}</h3>
      <b-card>
        <b-card-text>{{ play.message }}</b-card-text>
      </b-card>
      <b-container class="mt-3 p-0">
        <b-row>
          <b-col v-for="outcome in play.outcomes" :key="outcome.id">
            <NuxtLink :to="`/play/${play.id}/outcome/${outcome.id}`">
              <b-button variant="outline-primary" class="w-100 h-100">
                <b-icon :icon="outcome.icon"></b-icon>
                {{ outcome.description }}
              </b-button>
            </NuxtLink>
          </b-col>
        </b-row>
      </b-container>
      <!-- Breadcrumbs -->
      <b-container class="mt-3 p-0">
        <b-breadcrumb>
          <b-breadcrumb-item :href="item.path" :active="item.active" v-for="(item, i) in crumbs" :key="i">
            <!-- Com o NuxtLink, o breadcrumb active não muda o estilo e continua um link normal, então fiz essa condição pra quando o item estar ativo, usa a tag span -->
            <NuxtLink v-if="!item.active" :to="item.path">{{ item.text }}</NuxtLink>
            <span v-else>{{ item.text }}</span>
          </b-breadcrumb-item>
        </b-breadcrumb>
      </b-container>
    </b-card>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        // Montando o array de objetos com cada fase (play), suas mensagens e seus outcomes
        plays: [
          {
            id: 1,
            message: 'Você acordou na floresta tonto e com uma grande dor de cabeça, sem nenhuma memória de como foi parar ali. Um barulho cada vez mais intenso foi o que despertou você desse apagão. Após abrir os olhos e levantar-se, você observa uma manada de búfalos vindo em sua direção, a pouquíssimos metros de distância. O que você faz?',
            outcomes: [
              {
                id: 1,
                description: 'Corre o mais rápido possível',
                result: 'A sua corrida não foi suficiente para superar a da manada. Você morreu atropelado.',
                icon: 'chevron-double-right',
                correct: false
              },
              {
                id: 2,
                description: 'Sobe na árvore',
                result: 'Você sobe na árvore e escapa por pouco da manada.',
                icon: 'tree-fill',
                correct: true
              },
              {
                id: 3,
                description: 'Se finge de morto',
                result: 'Os búfalos podem até ter acreditado, mas isso não os impediu de passar por cima de você. Morreu atropelado.',
                icon: 'emoji-dizzy-fill',
                correct: false
              },
            ]
          },
          {
            id: 2,
            message: 'Ao ver que a manada passou, você desce da árvore e começa a explorar a região em busca de respostas. Não muito distante dali, você encontra um acampamento abandonado e um bilhete com instruções, deixado para trás. Antes que você possa alcançar o bilhete, um homem aparece com uma marreta gritando e avançando em sua direção. No seu corpo a adrenalina dispara. O que você faz?',
            outcomes: [
              {
                id: 1,
                description: 'Foge para bem longe',
                result: 'Você consegue fugir do local e voltar à cidade, porém sem nenhuma ideia de quem o atacou. Você continua tentando juntar as peças do quebra-cabeças. 2 semanas depois você morre atropelado, “acidentalmente”, ao sair de casa.',
                icon: 'peace',
                correct: false
              },
              {
                id: 2,
                description: 'Pega o bilhete e foge para bem longe',
                result: 'O homem lhe alcançou enquanto você tentava pegar o bilhete e lhe acertou uma marretada. Você morreu.',
                icon: 'card-text',
                correct: false
              },
              {
                id: 3,
                description: 'Puxa toda coragem do coração e enfrenta o homem',
                result: 'Você consegue desviar da primeira marretada do homem, que fica ocioso por um curto período, o suficiente para você acertá-lo em cheio na cabeça e apagá-lo no chão.',
                icon: 'emoji-angry-fill',
                correct: true
              },
            ]
          },
          {
            id: 3,
            message: 'Você finalmente consegue ler o bilhete: “Vamos fazer parecer um acidente. Misture o conteúdo deste frasco na comida dele. O remédio vai começar a fazer efeito depois de 30 minutos. Quando ele estiver completamente apagado, leve-o até o pé da árvore mais alta no início da floresta e direcione a manada de búfalos em sua direção. Estaremos esperando no laboratório ao norte do acampamento.” O que você faz?',
            outcomes: [
              {
                id: 1,
                description: 'Foge e liga para a polícia',
                result: 'Você informa a polícia o que aconteceu e passa a localização do laboratório. Chegando lá ela se depara com um sítio de produção de cocaína e prende todos os presentes. As evidências sugerem que você ainda pode estar em perigo, por este motivo a polícia lhe coloca no programa de proteção à testemunha e você sobrevive para vivenciar uma nova história. Parabéns, você ganhou!',
                icon: 'telephone-fill',
                victory: true
              },
              {
                id: 2,
                description: 'Amarra o homem e vai ao laboratório',
                result: ' Você amarra o homem e se dirige ao laboratório. Você entra silenciosamente e consegue chegar ao salão principal, onde você observa aproximadamente umas 10 pessoas. Um passo em falso entrega a sua posição e você é alvejado. Você morreu.',
                icon: 'lock-fill',
                correct: false
              },
              {
                id: 3,
                description: 'Mata o homem, pega a marreta e vai ao laboratório',
                result: 'Você mata o homem, pega a marreta e se dirige ao laboratório. Ao avistar 2 guardas na entrada, confiante de si e seus feitos recentes, você corre na direção dos guardas, segurando a marreta com as duas mãos levantadas acima da sua cabeça e gritando “LEEROY JEENKINS!!!!”. Infelizmente, o efeito surpresa foi prejudicado e os guardas não tiveram dificuldade em alvejá-lo. Você morreu.',
                icon: 'hammer',
                correct: false
              },
            ]
          }
        ]
      }
    },
    computed: {
      play() {
        // Baseado no id da url, esta função busca no array plays, o objeto play respectivo ao id. Este será passado para o child, para verificar o outcome do play atual
        return this.plays.find(p => p.id == this.$route.params.playid)
      },
      crumbs() {
        // Função para montar os breadcrumbs a partir do objeto plays
        const crumbs = []

        this.plays.forEach((item, i) => {
          // Se o id do item for maior do que o veio por parâmetro, deve sair do foreach. Os breadcrumbs não podem mostrar uma fase que não foi alcançada ainda.
          if(item.id > this.$route.params.playid) {
            return
          }

          const crumb = {}
          crumb.active = item.id == this.$route.params.playid ? true : false
          crumb.path = `/play/${item.id}`
          crumb.text = `Fase ${item.id}`
          
          crumbs.push(crumb)
        })

        return crumbs;
      }
    }
  }
</script>