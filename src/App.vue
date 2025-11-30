<script setup>
import { ref, computed } from 'vue';

const personas = ref([
    { nombre: "Daniel", apellido: "Sanchez", correo: "danielsanchez68@hotmail.com", dni: "20442873" },
    { nombre: "Juan", apellido: "Perez", correo: "j@p.gmail.com", dni: "12345678" },
    { nombre: "Ana", apellido: "Suarez", correo: "a@s.gmail.com", dni: "87654321" },
    { nombre: "Karleidys", apellido: "Gonzalez", correo: "k.gonzalez@ejemplo.com", dni: "98765432" },
]);

const filtroNombre = ref('');
const filtroDNI = ref('');


const getNombreCompleto = (persona) => {
    return `${persona.nombre} ${persona.apellido}`;
};


const mostrarAdvertencia = computed(() => {
    const largoNombre = filtroNombre.value.trim().length;
    const largoDNI = filtroDNI.value.trim().length;

    const nombreInvalido = largoNombre > 0 && largoNombre < 3;
    const dniInvalido = largoDNI > 0 && largoDNI < 3;

    return nombreInvalido || dniInvalido;
});

const personasFiltradas = computed(() => {
    const nombreFiltro = filtroNombre.value.trim().toLowerCase();
    const dniFiltro = filtroDNI.value.trim();

    if (mostrarAdvertencia.value) {
        return personas.value; 
    }
    
    if (nombreFiltro.length === 0 && dniFiltro.length === 0) {
        return personas.value;
    }

    return personas.value.filter(persona => {
        
        const cumpleNombre = nombreFiltro.length >= 3 
            ? (persona.nombre.toLowerCase().includes(nombreFiltro) || persona.apellido.toLowerCase().includes(nombreFiltro)) 
            : true; 

        const cumpleDNI = dniFiltro.length >= 3
            ? persona.dni.includes(dniFiltro) 
            : true; 
        
        return cumpleNombre && cumpleDNI;
    });
});
</script>

<template>
  <div class="container-fluid mt-3">
      <h2>Búsqueda de Personas</h2>
      
      <div class="row g-3 mb-3">
          <div class="col-md-6">
              <label for="filtro-nombre" class="form-label">Filtrar por Nombre/Apellido</label>
              <input type="text" id="filtro-nombre" class="form-control" v-model="filtroNombre" placeholder="Ingresar Nombre o Apellido...">
          </div>
          <div class="col-md-6">
              <label for="filtro-dni" class="form-label">Filtrar por DNI</label>
              <input type="text" id="filtro-dni" class="form-control" v-model="filtroDNI" placeholder="Ingresar DNI...">
          </div>
      </div>

      <div v-if="mostrarAdvertencia" class="alert alert-warning mb-4" role="alert">
          ⚠️ Debes ingresar al menos **3 caracteres** en el filtro activo para buscar.
      </div>
      
      <div class="card-deck m-0">
          <div class="row">
              <div class="col" v-for="persona in personasFiltradas" :key="persona.dni">
                  <div class="card mb-3">
                      <div class="card-body">
                          <h5 class="card-title">{{ getNombreCompleto(persona) }}</h5>
                          <p class="card-text">DNI: {{ persona.dni }}</p>
                          <a href="#" class="card-link">{{ persona.correo }}</a>
                      </div>
                  </div>
              </div>
          </div>
      </div>
  </div>
</template>

<style scoped>
.container-fluid {
    background-color: #f8f9fa; 
    padding-top: 30px;
    padding-bottom: 50px;
    min-height: 100vh;
}

h2 {
    color: #343a40; 
    margin-bottom: 25px;
    border-bottom: 2px solid #007bff; 
    padding-bottom: 10px;
    display: inline-block; 
}

.form-label {
    font-weight: bold;
    color: #495057;
}

.form-control {
    transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

.form-control:focus {
    border-color: #007bff;
    box-shadow: 0 0 0 0.25rem rgba(0, 123, 255, 0.25); 
}

.card {
    border-left: 5px solid #17a2b8; 
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); 
    transition: transform 0.2s ease;
    height: 100%;
}

.card:hover {
    transform: translateY(-5px); 
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.15); 
}

.card-title {
    color: #007bff; 
    font-weight: 600;
}

.card-text {
    font-size: 0.9em;
    color: #6c757d; 
    margin-bottom: 5px;
}

.card-link {
    color: #28a745; 
    text-decoration: none;
}

.card-deck .row > .col {
    margin-bottom: 15px; 
}
</style>
