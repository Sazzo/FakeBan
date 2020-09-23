<template>
  <div class="container">
    <TrollFace />
    <form onsubmit="return false" @submit-prevent="createAndSend()">
      <p v-if="error">
        {{ error }}
      </p>
      <div class="field">
        <label class="label">Qual é o bot desejado?</label>
        <div class="control">
          <div class="select">
            <select v-model="bot">
              <option>Loritta</option>
              <option>UnbeliavaBoat</option>
              <option>MEE6</option>
              <option>Custom</option>
            </select>
          </div>
        </div>
      </div>
      <div v-if="bot" class="fancyFields">
        <div class="field">
          <label class="label">URL da Webhook</label>
          <div class="control">
            <input v-model="url" class="input" type="text" placeholder="URL da Webhook" required="true">
          </div>
        </div>
        <div v-if="bot === `Custom`" class="optional">
          <div class="field">
            <label class="label">Nome da Webhook</label>
            <div class="control">
              <input v-model="name" class="input" type="text" placeholder="Nome da Webhook" required="true">
            </div>
          </div>
          <div class="field">
            <label class="label">Avatar da Webhook</label>
            <div class="control">
              <input v-model="avatar" class="input" type="url" placeholder="Avatar da Webhook" required="true">
            </div>
          </div>
          <div class="field">
            <label class="label">Mensagem</label>
            <div class="control">
              <input v-model="message" class="input" type="text" placeholder="Mensagem" required="true">
            </div>
          </div>
        </div>
        <div v-if="bot === `MEE6`" class="mee6Fields">
          <div class="field">
            <label class="label">Avatar do Usuário</label>
            <div class="control">
              <input v-model="UserAvatar" class="input" type="url" placeholder="Avatar do Usuário" required="true">
            </div>
          </div>
          <div class="field">
            <label class="label">Razão</label>
            <div class="control">
              <input v-model="reason" class="input" type="text" placeholder="Razão" required="true">
            </div>
          </div>
        </div>
        <div class="field" v-if="bot != `Custom`">
          <label v-if="bot != `MEE6`" class="label">Usuário a ser banido</label>
          <label v-else class="label">Usuário a ser banido (Exemplo: sazz#0002)</label>
          <div v-if="bot != `MEE6`" class="control">
            <input v-model="userId" class="input" type="number" placeholder="ID do Usuário" required="true">
          </div>
          <div v-else class="control">
            <input v-model="userTag" class="input" type="text" placeholder="Username#Tag" required="true">
          </div>
        </div>
        <button class="button" type="submit" @click="createAndSend">
          Enviar
        </button>
      </div>
      <p>💖 Created By <a href="https://sazz.fail">FelipeSazz</a></p>
      <p>🛠 Código Fonte: <a href="https://github.com/Sazzo/FakeBan">Github</a></p>
    </form>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { Webhook, MessageBuilder } from 'discord-webhook-node'
import { Constants } from '../utils/Constants'
export default Vue.extend({
  data () {
    return {
      url: '',
      name: '',
      avatar: '',
      userId: '',
      bot: '',
      reason: '',
      message: '',
      UserAvatar: '',
      userTag: '',
      error: ''
    }
  },
  methods: {
    async createAndSend () {
      try {
        if (this.bot === 'MEE6') {
          if (!this.UserAvatar || !this.userTag) {
            return
          }
        } else if (this.bot === 'UnbeliavaBoat' || this.bot === 'Loritta') {
          if (!this.userId) {
            return
          }
        } else if (this.bot === 'Custom') {
          if (!this.avatar || !this.name || !this.message) {
            return
          }
        }
        const webhook = new Webhook(this.url)
        switch (this.bot) {
          case 'Loritta': {
            webhook.setAvatar(Constants.LORITTA_ICON)
            webhook.setUsername('Loritta')
            await webhook.send(Constants.LORITTA_MESSAGE.replace('$USERID', this.userId))
            break
          }
          case 'UnbeliavaBoat': {
            webhook.setAvatar(Constants.UNBELIEVA_ICON)
            webhook.setUsername('UnbeliavaBoat')
            await webhook.send(Constants.UNBELIEVA_MESSAGE.replace('$USERID', this.userId).replace('$CASE', '`Case #9458`'))
            break
          }
          case 'MEE6': {
            webhook.setAvatar(Constants.MEE6_ICON)
            webhook.setUsername('MEE6')
            const meeEmbed = new MessageBuilder()
            meeEmbed.setAuthor(Constants.MEE6_EMBED.title.replace('$USERID', this.userTag), this.UserAvatar)
            meeEmbed.setDescription(Constants.MEE6_EMBED.description.replace('$REASON', this.reason))
            await webhook.send(meeEmbed)
            break
          }
          case 'Custom': {
            webhook.setAvatar(this.avatar)
            webhook.setUsername(this.name)
            await webhook.send(this.message)
            break
          }
        }
        // await webhook.send(`<:_check:634831134571888641> \`Case #9458\` <@${this.userId}> has been banned.`)
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
</style>
