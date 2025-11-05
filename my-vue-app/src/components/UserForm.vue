<template>
  <div>
    <h2>Adicionar Usuário</h2>
    <form @submit.prevent="addUser">
      <input v-model="name" placeholder="Nome" />
      <input v-model="email" placeholder="Email" />
      <button type="submit">Adicionar</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return { name: '', email: '' }
  },
  methods: {
    async addUser() {
      const newUser = { name: this.name, email: this.email }
      await this.$http.post('/users', newUser)
      alert('Usuário criado com sucesso!')
      this.name = ''
      this.email = ''
    }
  }
}
</script>