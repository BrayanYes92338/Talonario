<template>
  <div>
    <header class="header">
      <h2 class="titulo-header">TALONARIO</h2>
    </header>
    <main>
      <div class="modal-intro" v-if="modal_intro">
        <div class="intro">
          <div class="color-titulo">
            <h2>CONFIGURA TU TALONARIO</h2>
          </div>
          <div class="input-group">
            <input type="tel" placeholder="Ingrese el premio de la rifa" v-model="vrifa">

            <input type="tel" placeholder="Ingrese valor de la boleta" v-model="vboleta">
            <select name="" id="" v-model="loterias">
              <option disabled selected hidden value="">Seleccione la loteria </option>
              <option value="Cruz Roja">Cruz Roja</option>
              <option value="La Perla">La Perla</option>
              <option value="Santander">Santander</option>
              <option value="Baloto">Baloto</option>
            </select>
            <select name="" id="" v-model="cantboletas">
              <option disabled selected hidden value="">Cantidad de Boletas</option>
              <option value="0-99">0-99</option>
              <option value="0-999">0-999</option>
            </select>
            <input type="date" placeholder="Fecha de sorteo" :min="fechaMinima" v-model="fecha">
            <p v-if="error != ''">{{ error }}</p>
            <button class="button" @click="validar()">Guardar</button>
          </div>

        </div>
      </div>
      <div class="contenedor">
        <div class="cont-informacion">
          <h3 class="titulo-info">Información del Talonario</h3>
          <div class="cuerpo-info">
            <p class="icon">🏆<span>{{ talonario.vrifa }}</span></p>
            <p class="icon">💲<span>{{ talonario.vboleta }}</span></p>
            <p class="icon">🏦<span>{{ talonario.loterias }}</span></p>
            <p class="icon">🗓️<span>{{ talonario.fecha }}</span></p>
            <div class="boton">
              <button class="btn-editar"> Editar<i class="fa fa-edit"></i></button>
            </div>
          </div>
        </div>
        <div class="cont-cantboletas"></div>
        <div class="cont-acciones">
          <h3 class="titulo-info">Acciones</h3>
          <div class="cuerpo-acciones">
            <button class="btn-acciones"><i class="fa fa-list-ul"></i>LISTAR BOLETAS</button>
            <button class="btn-acciones"><i class="fa fa-cogs"></i>PERSONALIZAR</button>
            <button class="btn-acciones"><i class="fa fa-download"></i>GENERAR ARCHIVO</button>
          </div>
        </div>
      </div>
    </main>
    <footer class="footer">
      <div>
        <P>Copyright ©2024. Todos los derechos reservados.</P>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref } from "vue"
let datostalonario = ref([]);
let modal_intro = ref(true);
let vrifa = ref("");
let vboleta = ref("");
let loterias = ref("");
let cantboletas = ref("");
let fecha = ref("");
let error = ref("");
let talonario = ref({ vrifa: '', vboleta: '', loterias: '', fecha: '' });


function formatCurrency(amount) {
  return amount.toLocaleString('es-ES', { style: 'currency', currency: 'COP' });
}

function validar() {
  let fecha_actual = new Date()
  let fecha_select = new Date(fecha.value);

  if (vboleta.value == "") {
    error.value = "El valor de la boleta es requerido"
    setTimeout(() => {
      error.value = ""
    }, 5000);
  } else if (vboleta.value <= 0) {
    error.value = "El valor de la boleta no puede ser menor o igual a 0"
    setTimeout(() => {
      error.value = ""
    }, 5000);
  } else if (isNaN(vboleta.value)) {
    error.value = "El valor de la boleta debe ser numerico"
    setTimeout(() => {
      error.value = ""
    }, 5000);
  } else if (vrifa.value == "") {
    error.value = "El valor de la rifa es requerido"
    setTimeout(() => {
      error.value = ""
    }, 5000);
  } else if (vrifa.value <= 0) {
    error.value = "El valor de la rifa no puede ser menor o igual a 0"
    setTimeout(() => {
      error.value = ""
    }, 5000);
  }
  else if (isNaN(vrifa.value)) {
    error.value = "El valor de la rifa debe ser numerico"
    setTimeout(() => {
      error.value = ""
    }, 5000);

  } else if (loterias.value == "") {
    error.value = "Seleccione la loteria que quieras jugar"
    setTimeout(() => {
      error.value = ""
    }, 5000);
  } else if (cantboletas.value == "") {
    error.value = "Seleccione la cantidad de boletas"
    setTimeout(() => {
      error.value = ""
    }, 5000);
  } else if (fecha.value == "") {
    error.value = "Seleccione la fecha del sorteo"
    setTimeout(() => {
      error.value = ""
    }, 5000);
  } else if (fecha_actual > fecha_select) {
    error.value = "La fecha del sorteo no puede ser menor a la fecha actual"
    setTimeout(() => {
      error.value = ""
    }, 5000);

  } else {
    // form()
    actualizarInformacionTalonario()
    limpiar()
    modal_intro.value = false
  }
}

function actualizarInformacionTalonario() {
  
  talonario.value = {
    vrifa: formatCurrency(vrifa.value),
    vboleta: formatCurrency(vboleta.value),
    loterias: loterias.value,
    fecha: fecha.value,
  };
}

function form() {
  const registrodatos = {
    vrifa: formatCurrency(vrifa.value),
    vboleta: formatCurrency(vboleta.value),
    loterias: loterias.value,
    fecha: fecha.value,
  }

  datostalonario.value.push(registrodatos)
}

function limpiar() {
  vrifa.value = "";
  vboleta.value = "";
  loterias.value = "";
  cantboletas.value = "";
  fecha.value = "";

}


</script>