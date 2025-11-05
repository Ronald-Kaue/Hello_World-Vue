<!-- <script setup>
import { reactive, ref, computed } from 'vue'

const estado = reactive({
  nextId: 1,
  itens: []
})

const novoTexto = ref('')
const filtro = ref('')
const erro = ref('')

function adicionar() {
  const texto = novoTexto.value.trim()
  if (!texto) {
    erro.value = 'O campo não pode estar vazio.'
    return
  }

  erro.value = ''
  estado.itens.push({
    id: estado.nextId++,
    texto
  })
  novoTexto.value = ''
}

function remover(id) {
  estado.itens = estado.itens.filter(i => i.id !== id)
}

function limpar() {
  estado.itens = []
}

const filtradas = computed(() => {
  const termo = filtro.value.trim().toLowerCase()
  if (!termo) return estado.itens
  return estado.itens.filter(i => i.texto.toLowerCase().includes(termo))
})
</script>

<template>
  <form @submit.prevent="adicionar">
    <input
      v-model.trim="novoTexto"
      placeholder="Digite a mensagem"
      @input="erro = ''"
    />
    <button :disabled="!novoTexto">Adicionar</button>
  </form>

  <p v-if="erro" style="color: red; font-size: 0.9rem;">{{ erro }}</p>
  <input v-model.trim="filtro" placeholder="Filtrar mensagens" />
  <button @click="limpar" :disabled="!estado.itens.length">Limpar Tudo</button>
  <p v-if="estado.itens.length">
    Exibindo {{ filtradas.length }} de {{ estado.itens.length }}
  </p>

  <ul v-if="filtradas.length">
    <li
      v-for="m in filtradas"
      :key="m.id"
      :class="{ longo: m.texto.length > 30 }"
    >
      #{{ m.id }} — {{ m.texto }}
      <button @click="remover(m.id)">Remover</button>
    </li>
  </ul>
  <p v-else>Lista vazia ou nenhum resultado encontrado.</p>
</template>

<style scoped>
.longo {
  color: darkblue;
  font-weight: bold;
  background: #eef6ff;
  border-left: 4px solid #1e88e5;
  padding-left: 6px;
}
</style> -->

<template>
  <div>
    <h2>Usuários</h2>

    <ul v-if="users.length">
      <li v-for="u in users" :key="u.id">
        {{ u.name }} — {{ u.email }}
        <button @click="deleteUser(u.id)">🗑️ Excluir</button>
      </li>
    </ul>

    <p v-else>Nenhum usuário encontrado.</p>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      users: [
    { "id": 1, "name": "Ana", "email": "ana@email.com" },
    { "id": 2, "name": "Bruno", "email": "bruno@email.com" }
  ]
    }
  },
  async created() {
    try {
      // Busca na fake API (json-server)
      const res = await axios.get('http://localhost:3000/users')
      this.users = res.data
    } catch (err) {
      console.error('Erro ao carregar usuários:', err)
    }
  },
  methods: {
    async deleteUser(id) {
      try {
        await axios.delete(`http://localhost:3000/users/${id}`)
        this.users = this.users.filter(u => u.id !== id)
      } catch (err) {
        console.error('Erro ao excluir usuário:', err)
      }
    }
  }
}
</script>