<template>
  <div>
    <header class="header"
      :style="{ backgroundColor: colorheader === '1' ? '#F1B300' : colorheader === '2' ? '#78A036' : colorheader === '3' ? '#BD5288' : colorheader === '4' ? '#F6B363' : '#014BAE' }">
      <h2 class="titulo-header">TALONARIO</h2>
    </header>
    <main>
      <div class="modal-intro" v-if="modal_intro">
        <div class="intro">
          <div class="color-titulo"  :style="{ backgroundColor: colorheader === '1' ? '#F1B300' : colorheader === '2' ? '#78A036' : colorheader === '3' ? '#BD5288' : colorheader === '4' ? '#F6B363' : '#014BAE' }">
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
            <button class="buttonr" @click="validar()" :style="{ backgroundColor: colorbotones === '1' ? '#F1B300' : colorbotones === '2' ? '#78A036' : colorbotones === '3' ? '#BD5288' : colorbotones === '4' ? '#F6B363' : '#014BAE' }">Guardar</button>
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
              <button class="btn-editar" @click="editar(item, i)" :style="{ backgroundColor: colorbotones === '1' ? '#F1B300' : colorbotones === '2' ? '#78A036' : colorbotones === '3' ? '#BD5288' : colorbotones === '4' ? '#F6B363' : '#014BAE' }"> Editar<i class="fa fa-edit"></i></button>
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
            <button class="btn-acciones" @click="listardatos()" :style="{ backgroundColor: colorbotones === '1' ? '#F1B300' : colorbotones === '2' ? '#78A036' : colorbotones === '3' ? '#BD5288' : colorbotones === '4' ? '#F6B363' : '#014BAE' }"><i class="fa fa-list-ul" ></i>LISTAR BOLETAS</button>
            <button class="btn-acciones" @click="aparecerpersonalizar()" :style="{ backgroundColor: colorbotones === '1' ? '#F1B300' : colorbotones === '2' ? '#78A036' : colorbotones === '3' ? '#BD5288' : colorbotones === '4' ? '#F6B363' : '#014BAE' }"><i class="fa fa-cogs"></i>PERSONALIZAR</button>
            <button class="btn-acciones" @click="Ganador()" :style="{ backgroundColor: colorbotones === '1' ? '#F1B300' : colorbotones === '2' ? '#78A036' : colorbotones === '3' ? '#BD5288' : colorbotones === '4' ? '#F6B363' : '#014BAE' }" ><i class="fa fa-trophy"></i>GANADOR</button>
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
                <h6 class="rowc">Estado: Pagado <div class="color" style="background-color: #18BF67;"></div>
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
              <input type="tel" required pattern="[0-9]+" maxlength="10" placeholder="Ingrese numero telefonico "
                v-model="telefonoC">
              <select v-model="estadoC">
                <option disabled selected hidden value="">Seleccione el estado de la boleta</option>
                <option value="1">Apartado</option>
                <option value="2">Pagado</option>
                <option value="3">Ganador</option>
              </select>
              <p v-if="error2 != ''">{{ error2 }}</p>
              <button class="btn btn-primary botoncito" @click="validarcliente()">Registrar</button>
            </div>
          </div>
        </div>
      </div>

      <div class="modal fade" id="participante" tabindex="-1" aria-labelledby="exampleModalLabel2" aria-hidden="true">
        <div class="modal-dialog modal-dialog-centered modal-bottom">
          <div class="modal-content">
            <div class="modal-header  encabezado"  :style="{ backgroundColor: colorheader === '1' ? '#F1B300' : colorheader === '2' ? '#78A036' : colorheader === '3' ? '#BD5288' : colorheader === '4' ? '#F6B363' : '#014BAE' }">
              <h1 class="modal-title fs-5 titulo-datos-boleta titulo4" id="exampleModalLabel2">Participante</h1>
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
              <div class="cuerpo">
                <div class="nombre_cliente">
                  <i class="fa fa-user  icon1"></i>
                  <div class="c1">
                    <h6 class="mini">Nombre:</h6>
                    <span>{{ nombreP }}</span>
                  </div>
                </div>
                <div class="direccion_cliente">
                  <i class="fa fa-home icon4"></i>
                  <div class="c1">

                    <h6 class="mini">Dirección:</h6>
                    <span>{{ direccionP }}</span>
                  </div>
                </div>
                <div class="telefono_cliente">
                  <i class="fa fa-phone icon2"></i>
                  <div class="c1">
                    <h6 class="mini">Telefono:</h6>
                    <span>{{ telefonoP }}</span>
                  </div>
                </div>
                <div class="fechas_cliente">
                  <i class="fa fa-calendar icon3"></i>
                  <div class="c1">
                    <h6 class="mini">Fecha:</h6>
                    <span>{{ fechaP }}</span>
                  </div>
                </div>
              </div>
              <div class="estados">
                <i class="fa fa-list icon5"></i>
                <div class="c1">
                  <h6 class="mini">Estado:</h6>
                  <span v-if="estado === 1">Sin Pagar</span>
                  <span v-if="estado === 2">Pagado</span>
                  <span v-if="estado === 3">Ganador</span>
                </div>
              </div>

            </div>
          </div>
        </div>
      </div>
      <div class="listar-boletas caja" v-if="divaparecer2">
        <div class="cont-listar">
          <div class="cont-conted">
            <div class="cont-listado-titulo"  :style="{ backgroundColor: colorheader === '1' ? '#F1B300' : colorheader === '2' ? '#78A036' : colorheader === '3' ? '#BD5288' : colorheader === '4' ? '#F6B363' : '#014BAE' }">
              <span class="closeicon " @click="cerrar2()"><i class="fa fa-times"></i></span>
              <h2>Listado de Boleta</h2>
            </div>
            <div class="cont-listado-cuerpo">
              <table id="tab">
                <thead>
                  <tr>
                    <th>Nombre Comprador</th>
                    <th>Dirrecion</th>
                    <th>Numero Telefonico</th>
                    <th>Fecha Compra Boleta</th>
                    <th>Estado Boleta</th>
                    <th>N. Boleta </th>
                    <th>Eliminar</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(item, i) in registros" :key="i">
                    <td>{{ item.nombre }}</td>
                    <td>{{ item.direccion }}</td>
                    <td>{{ item.telefono }}</td>
                    <td>{{ item.fecha }}</td>
                    <td>{{ item.estadoTexto }}</td>
                    <td>{{ item.boleta }}</td>
                    <td><button class="btn-eliminar" @click="eliminar(i)"> <i class="fa fa-trash"></i></button></td>
                  </tr>
                </tbody>
              </table>
            </div>
            <div class="contenido2">
              <button class="btn-acciones" @click="download()" :style="{ backgroundColor: colorbotones === '1' ? '#F1B300' : colorbotones === '2' ? '#78A036' : colorbotones === '3' ? '#BD5288' : colorbotones === '4' ? '#F6B363' : '#014BAE' }"><i class="fa fa-download"></i>GENERAR ARCHIVO</button>
            </div>
          </div>
        </div>
      </div>
      <div class="modal-temas" v-if="divaparecer">
        <div class="cont-temas">
          <div class="cont-conte2">
            <div class="titulo_temas" :style="{ backgroundColor: colorheader === '1' ? '#F1B300' : colorheader === '2' ? '#78A036' : colorheader === '3' ? '#BD5288' : colorheader === '4' ? '#F6B363' : '#014BAE' }">
              <span class="closeicon " @click="cerrar()"><i class="fa fa-times"></i></span>
              <h2>Temas Para El Talonario</h2>
            </div>
            <div class="cont-cuerpo">
              <div class="colores_header">
                <div class="titulo_temas2">
                  <h2>Colores Header o Encabezados</h2>
                </div>
                <div class="cont_color_header">
                  <div class="cont_header_amarillo bordercito">
                    <input type="radio" name="color" value="1" v-model.number="colorheader" id="color_amarillo">
                    <div class="cont-color">
                      <label for="color_amarillo">Color Amarillo</label>
                      <div class="color amarillo"></div>
                    </div>
                  </div>
                  <div class="cont_header_verde bordercito">
                    <input type="radio" name="color" value="2" v-model="colorheader" id="color_verde">
                    <div class="cont-color">
                      <label for="color_verde">Color Verde</label>
                      <div class="color verde"></div>
                    </div>
                  </div>
                  <div class="cont_header_rosa bordercito">
                    <input type="radio" name="color" value="3" v-model="colorheader" id="color_rojo">
                    <div class="cont-color">
                      <label for="color_rojo">Color Rosa</label>
                      <div class="color rosa"></div>
                    </div>
                  </div>
                  <div class="cont_header_naranja bordercito">
                    <input type="radio" name="color" value="4" v-model="colorheader" id="color_naranja">
                    <div class="cont-color">
                      <label for="color_morado">Color Naranja</label>
                      <div class="color naranja"></div>
                    </div>
                  </div>
                </div>
              </div>
              <div class="colores_footer">
                <div class="titulo_temas2">
                  <h2>Colores Footer</h2>
                </div>
                <div class="cont_color_header">
                  <div class="cont_header_amarillo bordercito">
                    <input type="radio" name="color" value="1" v-model="colorfooter" id="color_amarillo">
                    <div class="cont-color">
                      <label for="color_amarillo">Color Amarillo</label>
                      <div class="color amarillo"></div>
                    </div>
                  </div>
                  <div class="cont_header_verde bordercito">
                    <input type="radio" name="color" value="2" v-model="colorfooter" id="color_verde">
                    <div class="cont-color">
                      <label for="color_verde">Color Verde</label>
                      <div class="color verde"></div>
                    </div>
                  </div>
                  <div class="cont_header_rosa bordercito">
                    <input type="radio" name="color" value="3" v-model="colorfooter" id="color_rojo">
                    <div class="cont-color">
                      <label for="color_rojo">Color Rosa</label>
                      <div class="color rosa"></div>
                    </div>
                  </div>
                  <div class="cont_header_naranja bordercito">
                    <input type="radio" name="color" value="4" v-model="colorfooter" id="color_naranja">
                    <div class="cont-color">
                      <label for="color_morado">Color Naranja</label>
                      <div class="color naranja"></div>
                    </div>
                  </div>
                </div>
              </div>
              <div class="colores_pestañas">
                <div class="titulo_temas2">
                  <h2>Colores Botones</h2>
                </div>
                <div class="cont_color_header">
                  <div class="cont_header_amarillo bordercito">
                    <input type="radio" name="color" value="1" v-model="colorbotones" id="color_amarillo">
                    <div class="cont-color">
                      <label for="color_amarillo">Color Amarillo</label>
                      <div class="color amarillo"></div>
                    </div>
                  </div>
                  <div class="cont_header_verde bordercito">
                    <input type="radio" name="color" value="2" v-model="colorbotones" id="color_verde">
                    <div class="cont-color">
                      <label for="color_verde">Color Verde</label>
                      <div class="color verde"></div>
                    </div>
                  </div>
                  <div class="cont_header_rosa bordercito">
                    <input type="radio" name="color" value="3" v-model="colorbotones" id="color_rojo">
                    <div class="cont-color">
                      <label for="color_rojo">Color Rosa</label>
                      <div class="color rosa"></div>
                    </div>
                  </div>
                  <div class="cont_header_naranja bordercito">
                    <input type="radio" name="color" value="4" v-model="colorbotones" id="color_naranja">
                    <div class="cont-color">
                      <label for="color_morado">Color Naranja</label>
                      <div class="color naranja"></div>
                    </div>
                  </div>
                </div>
              </div>
              <div class="botones">
                <button class="btn-color" @click="restaurarColor()">Colores por Defecto</button>
                <button class="btn-closed" @click="cerrar()">Cerrar</button>
              </div>

            </div>
          </div>
        </div>
      </div>
    </main>
    <footer class="footer"
      :style="{ backgroundColor: colorfooter === '1' ? '#F1B300' : colorfooter === '2' ? '#78A036' : colorfooter === '3' ? '#BD5288' : colorfooter === '4' ? '#F6B363' : '#014BAE' }">
      <div>
        <p>Copyright ©2024. Todos los derechos reservados.</p>
      </div>
    </footer>
  </div>
</template>
<script setup>
import { ref } from "vue"
import jsPDF from 'jspdf';
import autoTable from 'jspdf-autotable';

let registros = ref([]);
let arr = ref([]);
let colores = ref([]);
let colorheader = ref("");
let colorfooter = ref("");
let colorbotones = ref("")
let datostalonario = ref([]);
let modal_intro = ref(true);
let vrifa = ref("");
let vboleta = ref("");
let loterias = ref("");
let cantboletas = ref("");
let fecha = ref("");
let edit = true;
let index = null;
let i = ref(0);
let numsele = ref(0);
let estado = ref(0);
let nombreC = ref("");
let direccionC = ref("");
let telefonoC = ref("");
let fechaC = ref("");
let estadoC = ref("");
let nombreP = ref("");
let direccionP = ref("");
let telefonoP = ref("");
let fechaP = ref("");
let divaparecer = ref(false);
let divaparecer2 = ref(false);
let error2 = ref("");


function Ganador() {
  const NGanador = Math.floor(Math.random() * 100) + 1;
  Swal.fire("El número ganador es: " + NGanador);
}


function restaurarColor() {
  const color = {
    colorheader: colorheader.value,
    colorfooter: colorfooter.value,
    colorbotones: colorbotones.value

  }
  colores.value.push(color)
  colorheader.value = ""
  colorfooter.value = ""
  colorbotones.value = ""


}

function download() {
  const doc = new jsPDF('landscape');
  let bodyData = [];
  let tableElement = document.getElementById('tab');
  let rows = tableElement.querySelectorAll('tr');


  for (let i = 1; i < rows.length; i++) {
    let row = rows[i];
    let cols = row.querySelectorAll('td');
    let rowData = [];
    for (let j = 0; j < cols.length; j++) {
      let col = cols[j];
      rowData.push(col.innerText);
    }
    bodyData.push(rowData);
  }

  let headers = ['Nombre Comprador', 'Direccion', 'Numero Telefonico', 'Fecha Compra Boleta', 'Estado Boleta', 'N. Boleta'];
  autoTable(doc, {
    head: [headers],
    body: bodyData,
  });
  doc.save('Registro.pdf');
}


function validarcliente() {

  let texto = /^[A-Za-zÁÉÍÓÚáéíóúñÑüÜ\s]+$/;
  let fechajuego = datostalonario.value[datostalonario.value.length - 1].fecha;
 

  if (nombreC.value == "") {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "El nombre del comprador es requerido",
      timer: 3500
    });

  } else if (!texto.test(nombreC.value)) {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "El campo de nombre comprador no puede llevar numeros",
      timer: 3500
    });
  } else if (direccionC.value == "") {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "La dirrecion del comprador es requerida",
      timer: 3500
    });
  } else if (telefonoC.value == "") {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "El telefono del comprador es requerido",
      timer: 3500
    });
  } else if (isNaN(telefonoC.value) == true) {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "El campo de telefono del comprador debe ser numerico",
      timer: 3500
    });
  } else if (telefonoC.value.length != 10) {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "El campo de telefono debe tener al menos 10 numeros",
      timer: 3500
    });
  }  else if (estadoC.value == "") {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "El estado de la compra de la boleta es requerido",
      timer: 3500
    });
  } else {
    regBoletas()
    limpiar2()

    Swal.fire({
      icon: "success",
      title: "Boleta vendida",
      showConfirmButton: false,
      timer: 1500
    });
  }


}


function regBoletas() {

  const estado = parseInt(estadoC.value);
  let estadoTexto = "";

  if (estado === 1) {
    estadoTexto = "Apartado";
  } else if (estado === 2) {
    estadoTexto = "Pagado";
  } else if (estado === 3) {
    estadoTexto = "Ganador";
  }

  const cliente = {
    nombre: nombreC.value,
    direccion: direccionC.value,
    telefono: telefonoC.value,
    fecha: new Date().toISOString().split('T')[0],
    estado: parseInt(estadoC.value),
    estadoTexto: estadoTexto,
    boleta: numsele.value
  }

  registros.value.push(cliente)

  arr.value[i.value].comprador = cliente
  console.log(arr.value);
  arr.value[i.value].estado = parseInt(estadoC.value)


}

function aparecerpersonalizar() {
  divaparecer.value = true

}

function cerrar() {
  divaparecer.value = false

}

function listardatos() {
  divaparecer2.value = true
}

function cerrar2() {
  divaparecer2.value = false

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
    if (vrifa.value == "") {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor del premio no puede estar vacio",
        timer: 3500
      });
    } else if (vrifa.value <= 0) {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor del premio no puede menor o igual a 0",
        timer: 3500
      });
    } else if (isNaN(vrifa.value)) {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor del premio debe ser numerico",
        timer: 3500
      });
    } else if (vboleta.value == "") {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor de la boleta no puede estar vacio",
        timer: 3500
      });
    } else if (vboleta.value <= 0) {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor de la boleta no puede ser menor o igual a 0",
        timer: 3500
      });
    }
    else if (isNaN(vboleta.value)) {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor de la boleta debe ser numerico",
        timer: 3500
      });
    }
     else if (loterias.value == "") {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "Seleccione la loteria que quieras jugar",
        timer: 3500
      });
    } else if (cantboletas.value == "") {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "Seleccione la cantidad de boletas",
        timer: 3500
      });
    } else if (fecha.value == "") {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "Seleccione la fecha del sorteo",
        timer: 3500
      });
    } else if (fecha_actual >= fecha_select) {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "La fecha seleccionada no puede ser anterior o igual a la fecha actual, ni puede ser la fecha de mañana. Por favor, seleccione una fecha posterior",
        timer: 5500
      });
    } else {
      Swal.fire({
        icon: "success",
        title: "Datos de loteria registrados",
        showConfirmButton: false,
        timer: 3500
      });

      agregar()

      modal_intro.value = false
    }
  } else {
    if (vrifa.value == "") {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor del premio no puede estar vacio",
        timer: 3500
      });
    } else if (vrifa.value <= 0) {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor del premio no puede menor o igual a 0",
        timer: 3500
      });
    } else if (isNaN(vrifa.value)) {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor del premio debe ser numerico",
        timer: 3500
      });
    } else if (vboleta.value == "") {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor de la boleta no puede estar vacio",
        timer: 3500
      });
    } else if (vboleta.value <= 0) {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor de la boleta no puede ser menor o igual a 0",
        timer: 3500
      });
    } else if (isNaN(vboleta.value)) {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "El valor de la boleta debe ser numerico",
        timer: 3500
      });
    }
    else if (loterias.value == "") {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "Seleccione la loteria que quieras jugar",
        timer: 3500
      });
    } 
    else if (cantboletas.value == "") {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "Seleccione la cantidad de boletas",
        timer: 3500
      });
    } else if (fecha.value == "") {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "Seleccione la fecha del sorteo",
        timer: 3500
      });
    } else if (fecha_actual > fecha_select) {
      Swal.fire({
        icon: "error",
        title: "Oops...",
        text: "La fecha seleccionada no puede ser anterior o igual a la fecha actual, ni puede ser la fecha de mañana. Por favor, seleccione una fecha posterior",
        timer: 5500
      });

    } else {
      datostalonario.value[index].vrifa = vrifa.value;
      datostalonario.value[index].vboleta = vboleta.value;
      datostalonario.value[index].loterias = loterias.value;
      datostalonario.value[index].cantboletas = cantboletas.value;
      datostalonario.value[index].fecha = fecha.value;

      edit = true;
      limpiar();
      modal_intro.value = false;
      Swal.fire({
        icon: "success",
        title: "Datos de loteria editados",
        showConfirmButton: false,
        timer: 3500
      });
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

function limpiar2() {
  nombreC.value = "";
  direccionC.value = "";
  telefonoC.value = "";
  estadoC.value = "";

}

function eliminar(i) {
  registros.value.splice(i, 1)
}

</script>