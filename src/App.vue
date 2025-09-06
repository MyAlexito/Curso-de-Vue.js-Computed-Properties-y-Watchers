<template>
  <main style="max-width: 720px; margin: 2rem auto; font-family: system-ui, sans-serif;">
    <h1>To-Do List (Vue 3)</h1>

        <div class="entrada">
      <input
        v-model="nuevaTarea"
        placeholder="Nueva tarea"
        @keyup.enter="agregarTarea"
      />
      <button @click="agregarTarea">Agregar</button>
    </div>

    <p v-if="tareas.length === 0" class="muted">No hay tareas. ¡Agrega la primera!</p>

      <ul v-else class="lista">
      <TodoItem
        v-for="(t, i) in tareas"
        :key="i"
        :tarea="t"
        :index="i"
        @toggle="toggleTarea"
        @remove="eliminarTarea"
      />
    </ul>

    <p class="resumen">
      Total: <strong>{{ totalTareas }}</strong> ·
      Completadas: <strong>{{ tareasCompletadas }}</strong> ·
      Pendientes: <strong>{{ tareasPendientes }}</strong>
    </p>

    <button
      v-if="tareasCompletadas > 0"
      class="btn-secundario"
      @click="limpiarCompletadas"
    >
      Limpiar completadas
    </button>
  </main>
</template>

<script>
import TodoItem from './components/TodoItem.vue'

export default {
  name: 'App',
  components: { TodoItem },
  data() {
    return {
      nuevaTarea: '',
      tareas: []
    }
  },
  created() {
    const saved = localStorage.getItem('tareas')
    if (saved) {
      try {
        this.tareas = JSON.parse(saved)
      } catch (e) {
        console.warn('No se pudo parsear localStorage, se reinicia lista', e)
        this.tareas = []
      }
    }
  },
  computed: {
    totalTareas() {
      return this.tareas.length
    },
    tareasCompletadas() {
      return this.tareas.filter(t => t.completada).length
    },
    tareasPendientes() {
      return this.totalTareas - this.tareasCompletadas
    }
  },
  watch: {
       tareas: {
      handler(nuevas) {
        localStorage.setItem('tareas', JSON.stringify(nuevas))
      },
      deep: true
    }
  },
  methods: {
    agregarTarea() {
      const txt = this.nuevaTarea.trim()
      if (!txt) return
      this.tareas.push({ texto: txt, completada: false })
      this.nuevaTarea = ''
    },
    toggleTarea(index) {
      this.tareas[index].completada = !this.tareas[index].completada
    },
    eliminarTarea(index) {
      this.tareas.splice(index, 1)
    },
    limpiarCompletadas() {
      this.tareas = this.tareas.filter(t => !t.completada)
    }
  }
}
</script>

<style>
.entrada {
  display: flex;
  gap: .5rem;
  margin-bottom: 1rem;
}
input {
  flex: 1;
  padding: .5rem .7rem;
  border: 1px solid #ccc;
  border-radius: .6rem;
}
button {
  padding: .5rem .8rem;
  border: none;
  border-radius: .6rem;
  cursor: pointer;
}
.btn-secundario {
  margin-top: .5rem;
}
.lista {
  list-style: none;
  padding: 0;
  margin: .5rem 0 0;
}
.muted {
  opacity: .7;
}
.resumen {
  margin-top: 1rem;
}
</style>
