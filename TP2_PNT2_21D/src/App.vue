<template>
  <div class="container mt-4">
    <h1>Buscador de Personas</h1>

    <input
      type="text"
      class="form-control mb-2"
      placeholder="Buscar por nombre o apellido"
      v-model="filtroNombre"
    />

    <input
      type="text"
      class="form-control mb-2"
      placeholder="Buscar por DNI"
      v-model="filtroDni"
    />

    <div v-if="mostrarAdvertencia" class="alert alert-warning mt-3">
      ⚠️ Debes ingresar al menos 3 caracteres en alguno de los filtros.
    </div>

    
    <div class="row row-cols-1 row-cols-md-3 g-4 mt-3">
      <div class="col" v-for="persona in personasFiltradas" :key="persona.dni">
        <div class="card h-100">
          <div class="card-body">
            <h5 class="card-title">{{ getNombreCompleto(persona) }}</h5>
            <p class="card-text">DNI: {{ persona.dni }}</p>
            <a href="#" class="card-link">{{ persona.correo }}</a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      filtroNombre: "",
      filtroDni: "",
     personas: [
        {
          nombre: "Daniel",
          apellido: "Sanchez",
          correo: "danielsanchez68@hotmail.com",
          dni: "20442873",
        },
        {
          nombre: "Juan",
          apellido: "Perez",
          correo: "j@p.gmail.com",
          dni: "12345678",
        },
        {
          nombre: "Ana",
          apellido: "Suarez",
          correo: "a@s.gmail.com",
          dni: "87654321",
        },
        {
          nombre: "Matías",
          apellido: "Gaya",
          correo: "matutegaya@gmail.com",
          dni: "46287967",
        },
      ],
    };
  },
  computed: {
    mostrarAdvertencia() {
      return (
        (this.filtroNombre.length > 0 && this.filtroNombre.length < 3) ||
        (this.filtroDni.length > 0 && this.filtroDni.length < 3)
      );
    },
    personasFiltradas() {
      if (this.mostrarAdvertencia) return [];

      if (!this.filtroNombre && !this.filtroDni) return this.personas;

      if (this.filtroNombre && !this.filtroDni) {
        return this.personas.filter((p) =>
          `${p.nombre} ${p.apellido}`.toLowerCase().includes(this.filtroNombre.toLowerCase())
        );
      }

      if (this.filtroDni && !this.filtroNombre) {
        return this.personas.filter((p) => p.dni.includes(this.filtroDni));
      }

      return this.personas.filter(
        (p) =>
          `${p.nombre} ${p.apellido}`.toLowerCase().includes(this.filtroNombre.toLowerCase()) &&
          p.dni.includes(this.filtroDni)
      );
    },
  },
  methods: {
    getNombreCompleto(p) {
      return `${p.nombre} ${p.apellido}`;
    },
  },
};
</script>
