<script setup>
import { reactive, ref } from 'vue'

const estado = reactive({
  nextId: 1,
  itens: []
})

const novoTexto = ref('')

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
</script>

<template>
  <form @submit.prevent="adicionar">
    <input v-model.trim="novoTexto" placeholder="Digite a mensagem" />
    <button>Adicionar</button>
  </form>

  <button @click="limpar" :disabled="!estado.itens.length">Limpar Tudo</button>

  <ul v-if="estado.itens.length">
    <li v-for="m in estado.itens" :key="m.id">
      #{{ m.id }} — {{ m.texto }}
      <button @click="remover(m.id)">Remover</button>
    </li>
  </ul>

  <p v-else>Lista vazia.</p>
</template>