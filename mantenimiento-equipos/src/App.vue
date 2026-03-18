<template>
  <div class="container">
    <header class="hero">
      <h1>{{ titulo }}</h1>
      <p>{{ subtitulo }}</p>
    </header>

    <section class="card">
      <h2>Registrar equipo</h2>

      <div class="grid">
        <div class="field">
          <label for="cliente">Nombre del cliente</label>
          <input
            id="cliente"
            type="text"
            v-model.trim="cliente"
            placeholder="Ej: Juan Pérez"
          />
        </div>

        <div class="field">
          <label for="tipoEquipo">Tipo de equipo</label>
          <select id="tipoEquipo" v-model="tipoEquipo">
            <option value="">Seleccione una opción</option>
            <option>Laptop</option>
            <option>PC de escritorio</option>
            <option>Impresora</option>
            <option>Monitor</option>
            <option>Otro</option>
          </select>
        </div>

        <div class="field">
          <label for="marca">Marca</label>
          <input
            id="marca"
            type="text"
            v-model.trim="marca"
            placeholder="Ej: HP, Dell, Lenovo"
          />
        </div>

        <div class="field">
          <label for="modelo">Modelo</label>
          <input
            id="modelo"
            type="text"
            v-model.trim="modelo"
            placeholder="Ej: Inspiron 15"
          />
        </div>

        <div class="field full">
          <label for="falla">Falla reportada</label>
          <textarea
            id="falla"
            v-model.trim="falla"
            placeholder="Describa la falla del equipo"
          ></textarea>
        </div>

        <div class="field">
          <label for="fechaIngreso">Fecha de ingreso</label>
          <input id="fechaIngreso" type="date" v-model="fechaIngreso" />
        </div>

        <div class="field">
          <label for="estado">Estado inicial</label>
          <select id="estado" v-model="estado">
            <option>Recibido</option>
            <option>En diagnóstico</option>
            <option>En reparación</option>
            <option>Reparado</option>
            <option>Entregado</option>
          </select>
        </div>

        <div class="field">
          <label for="costoRevision">Costo de revisión ($)</label>
          <input
            id="costoRevision"
            type="number"
            min="0"
            step="0.01"
            v-model="costoRevision"
          />
        </div>

        <div class="field">
          <label for="costoReparacion">Costo de reparación ($)</label>
          <input
            id="costoReparacion"
            type="number"
            min="0"
            step="0.01"
            v-model="costoReparacion"
          />
        </div>

        <div class="field">
          <label for="costoRepuestos">Costo de repuestos ($)</label>
          <input
            id="costoRepuestos"
            type="number"
            min="0"
            step="0.01"
            v-model="costoRepuestos"
          />
        </div>
      </div>

      <div class="actions">
        <button @click="registrarEquipo">Registrar equipo</button>
        <button class="secondary" @click="limpiarFormulario">Limpiar</button>
      </div>

      <p class="error" v-if="mensajeError">{{ mensajeError }}</p>
      <p class="success" v-if="mensajeExito">{{ mensajeExito }}</p>
    </section>

    <section class="card">
      <h2>Resumen del sistema</h2>
      <div class="stats">
        <div class="stat">
          <h3>Total de registros</h3>
          <p>{{ equipos.length }}</p>
        </div>
        <div class="stat">
          <h3>Equipos reparados</h3>
          <p>{{ equiposReparados }}</p>
        </div>
        <div class="stat">
          <h3>Equipos entregados</h3>
          <p>{{ equiposEntregados }}</p>
        </div>
      </div>
    </section>

    <section class="card">
      <div class="section-header">
        <h2>Lista de mantenimientos</h2>
        <div class="actions compact">
          <button
            class="secondary"
            @click="borrarTodo"
            v-if="equipos.length > 0"
          >
            Borrar todos los registros
          </button>
        </div>
      </div>

      <p v-if="equipos.length === 0" class="empty">
        No hay equipos registrados todavía.
      </p>

      <div v-if="equipos.length > 0" class="table-wrapper">
        <table>
          <thead>
            <tr>
              <th>#</th>
              <th>Cliente</th>
              <th>Equipo</th>
              <th>Marca / Modelo</th>
              <th>Falla</th>
              <th>Fecha</th>
              <th>Estado</th>
              <th>Total</th>
              <th>Acciones</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(equipo, index) in equipos" :key="equipo.id">
              <td>{{ index + 1 }}</td>
              <td>{{ equipo.cliente }}</td>
              <td>{{ equipo.tipoEquipo }}</td>
              <td>{{ equipo.marca }} / {{ equipo.modelo }}</td>
              <td>{{ equipo.falla }}</td>
              <td>{{ equipo.fechaIngreso }}</td>
              <td>
                <select
                  v-model="equipo.estado"
                  :class="claseEstado(equipo.estado)"
                  @change="guardarDatos"
                >
                  <option>Recibido</option>
                  <option>En diagnóstico</option>
                  <option>En reparación</option>
                  <option>Reparado</option>
                  <option>Entregado</option>
                </select>
              </td>
              <td>${{ calcularTotal(equipo).toFixed(2) }}</td>
              <td class="btn-group">
                <button class="small" @click="seleccionarFactura(equipo)">
                  Ver factura
                </button>
                <button class="small danger" @click="eliminarEquipo(index)">
                  Eliminar
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>

    <section class="card factura" v-if="facturaSeleccionada">
      <h2>Factura de servicio</h2>

      <div
        v-if="
          facturaSeleccionada.estado === 'Reparado' ||
          facturaSeleccionada.estado === 'Entregado'
        "
      >
        <div class="invoice">
          <div class="invoice-header">
            <div>
              <h3>TechFix Solutions</h3>
              <p>Servicio técnico de computadoras</p>
            </div>
            <div class="invoice-number">
              <p><strong>Factura N.°:</strong> {{ facturaSeleccionada.id }}</p>
              <p><strong>Fecha:</strong> {{ fechaActual }}</p>
            </div>
          </div>

          <hr />

          <div class="invoice-info">
            <p><strong>Cliente:</strong> {{ facturaSeleccionada.cliente }}</p>
            <p><strong>Tipo de equipo:</strong> {{ facturaSeleccionada.tipoEquipo }}</p>
            <p><strong>Marca:</strong> {{ facturaSeleccionada.marca }}</p>
            <p><strong>Modelo:</strong> {{ facturaSeleccionada.modelo }}</p>
            <p><strong>Falla reportada:</strong> {{ facturaSeleccionada.falla }}</p>
            <p><strong>Estado actual:</strong> {{ facturaSeleccionada.estado }}</p>
          </div>

          <table class="invoice-table">
            <thead>
              <tr>
                <th>Concepto</th>
                <th>Costo</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>Revisión técnica</td>
                <td>${{ Number(facturaSeleccionada.costoRevision).toFixed(2) }}</td>
              </tr>
              <tr>
                <td>Reparación</td>
                <td>${{ Number(facturaSeleccionada.costoReparacion).toFixed(2) }}</td>
              </tr>
              <tr>
                <td>Repuestos</td>
                <td>${{ Number(facturaSeleccionada.costoRepuestos).toFixed(2) }}</td>
              </tr>
              <tr class="total-row">
                <td>Total a pagar</td>
                <td>${{ calcularTotal(facturaSeleccionada).toFixed(2) }}</td>
              </tr>
            </tbody>
          </table>

          <p class="thanks">Gracias por confiar en nuestro servicio técnico.</p>

          <div class="actions">
            <button @click="imprimirFactura">Imprimir factura</button>
            <button class="secondary" @click="cerrarFactura">
              Cerrar factura
            </button>
          </div>
        </div>
      </div>

      <div v-else class="warning-box">
        <p>
          No se puede generar la factura todavía. El equipo debe estar en estado
          <strong>“Reparado”</strong> o <strong>“Entregado”</strong>.
        </p>
      </div>
    </section>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, watch } from 'vue'

const STORAGE_KEY = 'mantenimiento-equipos-registros'

const titulo = ref('Sistema de Control de Mantenimiento de Equipos')
const subtitulo = ref(
  'Registro de reparaciones, control de estado y generación de factura.'
)

const cliente = ref('')
const tipoEquipo = ref('')
const marca = ref('')
const modelo = ref('')
const falla = ref('')
const fechaIngreso = ref('')
const estado = ref('Recibido')
const costoRevision = ref('')
const costoReparacion = ref('')
const costoRepuestos = ref('')

const equipos = ref([])
const facturaSeleccionada = ref(null)
const mensajeError = ref('')
const mensajeExito = ref('')

const equiposReparados = computed(() => {
  return equipos.value.filter((equipo) => equipo.estado === 'Reparado').length
})

const equiposEntregados = computed(() => {
  return equipos.value.filter((equipo) => equipo.estado === 'Entregado').length
})

const fechaActual = computed(() => {
  const hoy = new Date()
  return hoy.toLocaleDateString('es-SV')
})

const guardarDatos = () => {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(equipos.value))
}

const cargarDatos = () => {
  const datosGuardados = localStorage.getItem(STORAGE_KEY)
  if (datosGuardados) {
    equipos.value = JSON.parse(datosGuardados)
  }
}

const registrarEquipo = () => {
  mensajeError.value = ''
  mensajeExito.value = ''

  if (
    !cliente.value ||
    !tipoEquipo.value ||
    !marca.value ||
    !modelo.value ||
    !falla.value ||
    !fechaIngreso.value
  ) {
    mensajeError.value = 'Todos los campos obligatorios deben estar completos.'
    return
  }

  if (
    Number(costoRevision.value) < 0 ||
    Number(costoReparacion.value) < 0 ||
    Number(costoRepuestos.value) < 0
  ) {
    mensajeError.value = 'Los costos no pueden ser negativos.'
    return
  }

  const nuevoEquipo = {
    id: Date.now(),
    cliente: cliente.value,
    tipoEquipo: tipoEquipo.value,
    marca: marca.value,
    modelo: modelo.value,
    falla: falla.value,
    fechaIngreso: fechaIngreso.value,
    estado: estado.value,
    costoRevision: Number(costoRevision.value) || 0,
    costoReparacion: Number(costoReparacion.value) || 0,
    costoRepuestos: Number(costoRepuestos.value) || 0
  }

  equipos.value.push(nuevoEquipo)
  guardarDatos()
  mensajeExito.value = 'Equipo registrado correctamente.'
  limpiarFormulario()
}

const limpiarFormulario = () => {
  cliente.value = ''
  tipoEquipo.value = ''
  marca.value = ''
  modelo.value = ''
  falla.value = ''
  fechaIngreso.value = ''
  estado.value = 'Recibido'
  costoRevision.value = ''
  costoReparacion.value = ''
  costoRepuestos.value = ''
}

const calcularTotal = (equipo) => {
  return (
    Number(equipo.costoRevision) +
    Number(equipo.costoReparacion) +
    Number(equipo.costoRepuestos)
  )
}

const seleccionarFactura = (equipo) => {
  facturaSeleccionada.value = equipo
}

const cerrarFactura = () => {
  facturaSeleccionada.value = null
}

const eliminarEquipo = (index) => {
  const confirmar = window.confirm('¿Desea eliminar este registro?')
  if (confirmar) {
    equipos.value.splice(index, 1)
    guardarDatos()
    facturaSeleccionada.value = null
  }
}

const borrarTodo = () => {
  const confirmar = window.confirm(
    '¿Está seguro de borrar todos los registros guardados?'
  )
  if (confirmar) {
    equipos.value = []
    facturaSeleccionada.value = null
    localStorage.removeItem(STORAGE_KEY)
  }
}

const claseEstado = (estadoActual) => {
  if (estadoActual === 'Recibido') return 'estado-recibido'
  if (estadoActual === 'En diagnóstico') return 'estado-diagnostico'
  if (estadoActual === 'En reparación') return 'estado-reparacion'
  if (estadoActual === 'Reparado') return 'estado-reparado'
  if (estadoActual === 'Entregado') return 'estado-entregado'
  return ''
}

const imprimirFactura = () => {
  window.print()
}

watch(
  equipos,
  () => {
    guardarDatos()
  },
  { deep: true }
)

onMounted(() => {
  cargarDatos()
})
</script>