<template>
  <main style="max-width: 720px; margin: 2rem auto; font-family: system-ui, sans-serif;">
    <h1>Fundamentos: Computed vs Methods</h1>

    <!-- Input para nombre -->
    <label>
      Nombre:
      <input v-model="nombre" placeholder="Escribe tu nombre" />
    </label>

    <!-- Imagen con binding y tamaño ajustado -->
    <div style="margin-top:1rem;">
      <img 
        :alt="`Avatar de ${nombre || 'invitado'}`" 
        :src="'https://static.vecteezy.com/system/resources/previews/006/431/286/non_2x/hacker-behind-computer-hacking-password-in-laptop-man-in-hood-vector.jpg'"
      />
    </div>

    <!-- Propiedad computada -->
    <section style="margin-top: 1rem;">
      <h3>Con propiedad computada (cacheada)</h3>
      <p>{{ mensajeBienvenida }}</p>
      <p>{{ mensajeBienvenida }}</p>
    </section>

    <!-- Método -->
    <section style="margin-top: 1rem;">
      <h3>Con método (se ejecuta en cada render)</h3>
      <p>{{ saludoMetodo() }}</p>
      <p>{{ saludoMetodo() }}</p>
    </section>

    <!-- Contador de renders -->
    <p style="opacity:.7; margin-top:1rem;">Renders: {{ contadorRenders }}</p>
    <p style="opacity:.7;">Abre la consola del navegador para ver logs.</p>
  </main>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      nombre: '',
      contadorRenders: 0
    }
  },
  computed: {
    mensajeBienvenida() {
      console.log('[computed] mensajeBienvenida recalculado');
      return this.nombre ? `¡Bienvenido, ${this.nombre}!` : 'Bienvenido, invitado';
    }
  },
  methods: {
    saludoMetodo() {
      console.log('[method] saludoMetodo ejecutado');
      return this.nombre ? `Hola, ${this.nombre}` : 'Hola, invitado';
    }
  },
  updated() {
    this.contadorRenders++;
  }
}
</script>

<style>
input {
  margin-left: .5rem;
  padding: .4rem .6rem;
  border: 1px solid #ccc;
  border-radius: .5rem;
}

img {
  display: block;
  margin-top: .5rem;
  border-radius: 50%;
  width: 150px;       /* ancho fijo */
  height: 150px;      /* alto fijo */
  object-fit: cover;  /* mantiene proporciones y recorta si es necesario */
}
</style>
