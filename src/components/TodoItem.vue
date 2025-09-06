<template>
  <li class="tarea">
    <label>
      <input type="checkbox" v-model="tarea.completada" @change="$emit('toggle', index)" />
      <span :class="{ completada: tarea.completada }">{{ tarea.texto }}</span>
    </label>
    <button class="eliminar" @click="$emit('remove', index)">✕</button>
  </li>
</template>

<script>
export default {
  name: 'TodoItem',
  props: {
    tarea: Object,
    index: Number
  }
}
</script>

<style>
.tarea {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: .4rem .6rem;
  border-bottom: 1px solid #eee;
}
.tarea span.completada {
  text-decoration: line-through;
  color: #999;
}
.eliminar {
  background: transparent;
  border: none;
  color: red;
  font-weight: bold;
  cursor: pointer;
}
</style>
