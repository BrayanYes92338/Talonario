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
              <option value="">0-99</option>
              <option value="">0-999</option>
            </select>
            <input type="date" placeholder="Fecha de sorteo" v-model="fecha">
            <button class="button" @click="validar()">Guardar</button>
          </div>

        </div>

      </div>
      <div class="pdr">
        <div class="informacion">
          <h2 style="text-align: center;">INFORMACION</h2>
          <div class="inf" v-for="(item, i) in datostalonario" :key="i">
            <h3>🏆<span>{{ item.vrifa }}</span></h3>
            <h3>💲<span>{{ item.vboleta }}</span></h3>
            <h3>🏦<span>{{ item.loterias }}</span></h3>
            <h3>🗓️<span>{{ item.fecha }}</span></h3>
            <button class="but" @click="editar()">EDITAR</button>
          </div>
        </div>
        <table>
          <tbody>
            <td>

              <tr>
                <div class="boleta">0</div>
              </tr>
              <tr>
                <div class="boleta">01</div>
              </tr>
              <tr>
                <div class="boleta">02</div>
              </tr>
              <tr>
                <div class="boleta">03</div>
              </tr>
              <tr>
                <div class="boleta">04</div>
              </tr>
              <tr>
                <div class="boleta">05</div>
              </tr>
              <tr>
                <div class="boleta">06</div>
              </tr>
              <tr>
                <div class="boleta">07</div>
              </tr>
              <tr>
                <div class="boleta">08</div>
              </tr>
              <tr>
                <div class="boleta">09</div>
              </tr>
            </td>
          </tbody>
        </table>
        <div class="acciones">
          <h2 style="text-align: center;">ACCIONES</h2>
          <div class="acc">
            <button class="butt">LISTAR TUS BOLETAS</button>
            <button class="butt">PERSONALIZAR TALONARIO WEB</button>
            <button class="butt">GENERAR ARCHIVO DE DATOS</button>
            <div>
              <button class="re">✉</button>
              <button class="re">☏</button>
            </div>
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
let datostalonario = ref([])
let modal_intro = ref(true);
let vrifa = ref("");
let vboleta = ref("");
let loterias = ref("");
let cantboletas = ref("");
let fecha = ref("");

function formatCurrency(amount) {
  return amount.toLocaleString('es-ES', { style: 'currency', currency: 'COP' });
}


function validar() {
  modal_intro.value = false;
  form()
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

function editar() {
  modal_intro.value = false;
}


</script>