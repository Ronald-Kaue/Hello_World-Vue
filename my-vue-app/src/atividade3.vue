<script setup>
import { reactive, ref, computed } from 'vue'

const estado = reactive({
  nextId: 1,
  itens: []
})

const novoTexto = ref('')
const filtro = ref('')

function adicionar() {
  const texto = novoTexto.value.trim()
  if (!texto) return

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
    <input v-model.trim="novoTexto" placeholder="Digite a mensagem" />
    <button>Adicionar</button>
  </form>

  <input v-model.trim="filtro" placeholder="Filtrar mensagens" />

  <button @click="limpar" :disabled="!estado.itens.length">Limpar Tudo</button>

  <p v-if="estado.itens.length">
    Exibindo {{ filtradas.length }} de {{ estado.itens.length }}
  </p>

  <ul v-if="filtradas.length">
    <li v-for="m in filtradas" :key="m.id">
      #{{ m.id }} — {{ m.texto }}
      <button @click="remover(m.id)">Remover</button>
    </li>
  </ul>

  <p v-else>Lista vazia ou nenhum resultado encontrado.</p>
</template>
