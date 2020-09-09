<template>
  <div class="container">
    <TrollFace />
    <form onsubmit="return false" @submit-prevent="createAndSend">
      <p v-if="error">
        {{ error }}
      </p>
      <div class="field">
        <label class="label">URL da Webhook</label>
        <div class="control">
          <input v-model="url" class="input" type="text" placeholder="URL da Webhook" required="true">
        </div>
      </div>
      <div class="field">
        <label class="label">Nome da Webhook (Opcional)</label>
        <div class="control">
          <input v-model="name" class="input" type="text" placeholder="Nome da Webhook">
        </div>
      </div>
      <div class="field">
        <label class="label">Avatar da Webhook (Opcional)</label>
        <div class="control">
          <input v-model="avatar" class="input" type="url" placeholder="Avatar da Webhook">
        </div>
      </div>
      <div class="field">
        <label class="label">Usuário a ser banido</label>
        <div class="control">
          <input v-model="userId" class="input" type="number" placeholder="ID do Usuário">
        </div>
      </div>
      <button class="button" @click="createAndSend">
        Enviar
      </button>
      <p>PS: Isso vai enviar um fake  UnbeliavaBoat, em breve mais bots :3</p>
      <p>💖 Created By <a href="https://sazz.fail">FelipeSazz</a></p>
    </form>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { Webhook } from 'discord-webhook-node'
export default Vue.extend({
  data () {
    return {
      url: '',
      name: '',
      avatar: '',
      userId: '',
      error: ''
    }
  },
  methods: {
    async createAndSend () {
      try {
        if (!this.url) {
          this.error = 'Você precisa especificar um URL válido!'
          return
        }
        if (!this.userId) {
          this.error = 'Você precisa especificar um ID de Usuário!'
          return
        }
        const webhook = new Webhook(this.url)
        if (this.name) {
          webhook.setUsername(this.name)
        } else {
          webhook.setUsername('UnbeliavaBoat')
        }
        if (this.avatar) {
          webhook.setAvatar(this.avatar)
        } else {
          webhook.setAvatar('https://cdn.discordapp.com/avatars/292953664492929025/c47af6e9bae8a91779996fa412b24207.png?size=2048')
        }
        await webhook.send(`<:_check:634831134571888641> \`Case #9458\` <@${this.userId}> has been banned.`)
        this.error = 'Sucesso!'
      } catch (e) {
        this.error = e
      }
    }
  }
})

</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
