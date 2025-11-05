<template>
  <div>
    <h2>Posts</h2>

    <!-- Listagem -->
    <ul>
      <li v-for="p in posts" :key="p.id">
        <b>{{ p.title }}</b> — {{ p.body }}
        <button @click="startEdit(p)">✏️ Editar</button>
        <button @click="deletePost(p.id)">🗑️ Excluir</button>
      </li>
    </ul>

    <!-- Formulário de edição -->
    <div v-if="editing">
      <h3>Editar Post #{{ editData.id }}</h3>
      <input v-model="editData.title" placeholder="Título" />
      <textarea v-model="editData.body" placeholder="Conteúdo"></textarea>
      <button @click="updatePost">💾 Salvar</button>
      <button @click="cancelEdit">❌ Cancelar</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      posts: [],
      editing: false,
      editData: { id: null, title: '', body: '', userId: null }
    }
  },
  async created() {
    // Buscar posts ao carregar
    const res = await this.$http.get('/posts')
    this.posts = res.data
  },
  methods: {
    // Inicia edição
    startEdit(post) {
      this.editing = true
      this.editData = { ...post }
    },

    // Cancelar edição
    cancelEdit() {
      this.editing = false
      this.editData = { id: null, title: '', body: '', userId: null }
    },

    // Salvar alterações (PUT)
    async updatePost() {
      try {
        await this.$http.put(`/posts/${this.editData.id}`, this.editData)
        this.editing = false

        // Atualiza a lista após salvar
        const res = await this.$http.get('/posts')
        this.posts = res.data

        alert('Post atualizado com sucesso!')
      } catch (error) {
        alert('Erro ao atualizar o post!')
        console.error(error)
      }
    },

    // Excluir post
    async deletePost(id) {
      await this.$http.delete(`/posts/${id}`)
      this.posts = this.posts.filter(p => p.id !== id)
    }
  }
}
</script>

<style scoped>
ul {
  list-style: none;
  padding: 0;
}

input,
textarea {
  display: block;
  width: 100%;
  margin: 6px 0;
  padding: 8px;
}

button {
  margin: 4px;
}
</style>