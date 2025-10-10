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
</script>

<template>
  <form @submit.prevent="adicionar">
    <input v-model.trim="novoTexto" placeholder="Digite a mensagem" />
    <button>Adicionar</button>
  </form>

  <ul v-if="estado.itens.length">
    <li v-for="m in estado.itens" :key="m.id">
      #{{ m.id }} — {{ m.texto }}
    </li>
  </ul>
  <p v-else>Lista vazia.</p>
</template>