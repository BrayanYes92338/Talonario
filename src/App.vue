<template>
  <div>
    <header class="header">
      <h2 class="titulo-header">TALONARIO</h2>
    </header>
    <main>
      <div class="modal-intro" v-if="modal_intro">
        <div class="intro">
          <div class="color-titulo">
            <h2 class="titulo-info-talonario">CONFIGURA TU TALONARIO</h2>
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
              <option value="100">0-99</option>
              <option value="1000">0-999</option>
            </select>
            <input type="date" placeholder="Fecha de sorteo" v-model="fecha">
            <p class="error" v-if="error != ''">{{ error }}</p>
            <button class="buttonr" @click="validar()">Guardar</button>
          </div>

        </div>
      </div>
      <div class="contenedor">
        <div class="cont-informacion">
          <h3 class="titulo-info">Información del Talonario</h3>
          <div class="cuerpo-info" v-for="(item, i) in datostalonario" :key="i">
            <p class="icon">🏆<span>{{ item.vrifa }}</span></p>
            <p class="icon">💲<span>{{ item.vboleta }}</span></p>
            <p class="icon">🏦<span>{{ item.loterias }}</span></p>
            <p class="icon">🗓️<span>{{ item.fecha }}</span></p>
            <h6></h6>
            <div class="boton">
              <button class="btn-editar" @click="editar(item, i)"> Editar<i class="fa fa-edit"></i></button>
            </div>
          </div>
        </div>

        <div class="grid">
          <div class="cont-cantboletas">
            <div v-for="(item, i) in arr" :key="i">
              <button class="rounded-circle boleta" :style="item.estado === 0 ? 'background-color: #014BAE'
                : item.estado === 1 ? 'background-color: #CF1635'
                  : item.estado === 2 ? 'background-color: #0090A9'
                    : item.estado === 3 ? 'background-color: #BA8C34' : ''" type="button" data-bs-toggle="modal"
                data-bs-target="#exampleModal" @click="traerDatos(item, i)">{{ item.numero }}</button>
            </div>
          </div>
        </div>



        <div class="cont-acciones">
          <h3 class="titulo-info">Acciones</h3>
          <div class="cuerpo-acciones">
            <button class="btn-acciones"><i class="fa fa-list-ul"></i>LISTAR BOLETAS</button>
            <button class="btn-acciones"><i class="fa fa-cogs"></i>PERSONALIZAR</button>
            <button class="btn-acciones"><i class="fa fa-download"></i>GENERAR ARCHIVO</button>
          </div>
        </div>
      </div>
      <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered modal-bottom">
          <div class="modal-content">

            <div class="modal-body">
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
              <h3>Boleta {{ numsele }}</h3>
              <div v-if="estado === 0" class="cen">
                <h6 class="rowc">Estado: Disponible<div class="color" style="background-color: #00075F;"></div>
                </h6>
                <hr v-if="estado === 0">
                <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#staticBackdrop2">🤝
                  Adquirir Boleta </button>
              </div>
              <div v-if="estado === 1">
                <h6 class="rowc">Estado: Apartado <div class="color" style="background-color: #CF1635;"></div>
                </h6>
              </div>
              <div v-if="estado === 2">
                <h6 class="rowc">Estado: Pagado <div class="color" style="background-color: #0090A9;"></div>
                </h6>
              </div>
              <div v-if="estado === 3">
                <h6 class="rowc">Estado: Ganador <div class="color" style="background-color: #BA8C34;"></div>
                </h6>
              </div>
              <div v-if="estado !== 0" class="cen">
                <hr>
                <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#participante">👁‍🗨
                  Ver datos del participante </button>
                <br v-if="estado !== 3">
                <button v-if="estado !== 3" type="button" class="btn btn-primary" @click="libb()">❌ Liberar Boleta
                </button>
                <br v-if="estado !== 2 && estado !== 3">
                <button v-if="estado === 1" type="button" class="btn btn-primary" @click="pagar()"> ✔ Pagar Boleta
                </button>
              </div>

            </div>
          </div>
        </div>
      </div>
      <div class="modal fade prueba-color" id="staticBackdrop2" data-bs-backdrop="static" data-bs-keyboard="false"
        tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
        <div class="modal-dialog">
          <div class="modal-content contenido">
            <div class="modal-header ">
              <h1 class="modal-title fs-5 titulo-datos-boleta" id="staticBackdropLabel">Datos de Boleta</h1>
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
              <input type="text" placeholder="Ingrese nombre del comprador" v-model="nombreC">
              <input type="text" placeholder="Ingrese direccion del comprador" v-model="direccionC">
              <input type="number" placeholder="Ingrese numero telefonico " v-model="telefonoC">
              <input type="date" v-model="fechaC">
              <select v-model="estadoC">
                <option disabled selected hidden value="">Seleccione el estado de la boleta</option>
                <option value="1">Apartado</option>
                <option value="2">Pagado</option>
                <option value="3">Ganador</option>
              </select>
              <button class="btn btn-primary botoncito" @click="regBoletas()">Registrar</button>

            </div>
          </div>
        </div>
      </div>

      <div class="modal fade" id="participante" tabindex="-1" aria-labelledby="exampleModalLabel2" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered modal-bottom">
          <div class="modal-content">
            <div class="modal-header ">
              <h1 class="modal-title fs-5 titulo-datos-boleta" id="exampleModalLabel2">Participante</h1>
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">

              <span>{{ nombreP }}</span>
              <span>{{ direccionP }}</span>
              <span>{{ telefonoP }}</span>
              <span>{{ fechaP}}</span>
              <span v-if="estado === 1">Sin Pagar</span>
              <span v-if="estado === 2">Pagado</span>
              <span v-if="estado === 3">Ganador</span>

            </div>
          </div>
        </div>
      </div>
    </main>
    <footer class="footer">
      <div>
        <p>Copyright ©2024. Todos los derechos reservados.</p>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref } from "vue"
let registros = ref([]);
let arr = ref([]);
let datostalonario = ref([]);
let modal_intro = ref(true);
let vrifa = ref("");
let vboleta = ref("");
let loterias = ref("");
let cantboletas = ref("");
let fecha = ref("");
let error = ref("");
let edit = true;
let index = null;
let i = ref(0)
let numsele = ref(0)
let estado = ref(0)
let nombreC = ref("")
let direccionC = ref("")
let telefonoC = ref("")
let fechaC = ref("")
let estadoC = ref("");

let nombreP = ref("")
let direccionP = ref("")
let telefonoP = ref("")
let fechaP = ref("")



function regBoletas() {

  const cliente = {
    nombre: nombreC.value,
    direccion: direccionC.value,
    telefono: telefonoC.value,
    fecha: fechaC.value,
    estado: parseInt(estadoC.value)
  }

  registros.value.push(cliente)

  arr.value[i.value].comprador = cliente
  console.log(arr.value);
  arr.value[i.value].estado = parseInt(estadoC.value)

}

function traerDatos(item, index) {
  numsele.value = item.numero
  estado.value = item.estado
  i.value = index

  nombreP.value = item.comprador.nombre
  direccionP.value = item.comprador.direccion
  telefonoP.value = item.comprador.telefono
  fechaP.value = item.comprador.fecha
}

function libb() {

  arr.value[i.value].estado = 0
  arr.value[i.value].comprador = {}
}

function pagar() {
  arr.value[i.value].estado = 2

}


function validar() {
  let fecha_actual = new Date()
  let fecha_select = new Date(fecha.value);


  if (edit == true) {
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
      agregar()

      modal_intro.value = false
    }
  } else {
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
      datostalonario.value[index].vrifa = vrifa.value;
      datostalonario.value[index].vboleta = vboleta.value;
      datostalonario.value[index].loterias = loterias.value;
      datostalonario.value[index].cantboletas = cantboletas.value;
      datostalonario.value[index].fecha = fecha.value;

      edit = true;
      limpiar();
      modal_intro.value = false;
    }
  }

}


function agregar() {
  const talonario = {
    vrifa: vrifa.value,
    vboleta: vboleta.value,
    loterias: loterias.value,
    cantboletas: parseInt(cantboletas.value),
    fecha: fecha.value,
  };
  datostalonario.value.push(talonario);

  arr.value = Array.from({ length: cantboletas.value }, (value, index) => ({
    numero: index,
    estado: 0,
    comprador: {}
  }));

  console.log(arr.value);
  console.log(datostalonario.value);
  limpiar()
}


function editar(item, i) {
  console.log(item);
  console.log(i);
  vrifa.value = item.vrifa;
  vboleta.value = item.vboleta;
  loterias.value = item.loterias;
  fecha.value = item.fecha;
  edit = false;
  index = i;
  modal_intro.value = true;
}

function limpiar() {
  vrifa.value = "";
  vboleta.value = "";
  loterias.value = "";
  cantboletas.value = "";
  fecha.value = "";

}


</script>