<template>
  <div>
    <b-container>
      <b-row>
        <b-col>
          <TodoList />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import TodoList from '@/components/TodoList.vue'

export default {
  name: 'App',
  components: {
    TodoList
  }
}
</script>

<style>
.container {
  margin-top: 30px;
}
</style>
