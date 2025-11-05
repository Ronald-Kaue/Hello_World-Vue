<template>
  <div class="app">
    <h1>Fake API com Vue + json-server</h1>
    <UserForm />
    <UsersList />
    <PostList />
  </div>
</template>

<script>
import UserForm from './components/UserForm.vue'
import UsersList from './components/UsersList.vue'
import PostList from './components/PostList.vue'

export default {
  components: { UserForm, UsersList, PostList }
}
</script>
