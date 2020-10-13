<template>
  <b-container>
    <b-card>
      <b-row align-h="center" >
        <b-col sm="10" class="p-2" >
          <b-form-input
            id="input-1"
            v-model="search"
            placeholder="Search"
            trim >
          </b-form-input>
        </b-col>
        <b-col sm="2" class="p-2">
          <b-button
            variant="success"
            block
            @click="usuario_editable = { ...nuevo_usuario }, show = true"
          >
            <b-icon icon="plus-square-fill" class="pl-1" ></b-icon>
            Agregar
          </b-button>
        </b-col>
      </b-row>
      <b-table
        ref="tabla_datos"
        :items="items"
        :fields="fields"
        v-if="tablaVisible"
        small
        primary-key="id_usuario"
      >
        <template v-slot:cell(foto)="data">
          <b-img :src="data.item.foto" style="max-width: 70px; max-height: 70px" ></b-img>
        </template>
        <template v-slot:cell(_edit)="data">
          <b-img
            src="https://www.flaticon.com/svg/static/icons/svg/1160/1160515.svg"
            style="max-width: 25px; max-height: 25px; cursor: pointer"
            @click="editarUsuario(data.item.id_usuario)" ></b-img>
          <b-img
            src="https://www.flaticon.es/svg/static/icons/svg/1160/1160672.svg"
            class="pl-2"
            style="max-width: 25px; max-height: 25px; cursor: pointer"
            @click="eliminarUsuario(data.item.id_usuario)" ></b-img>
        </template>
      </b-table>
    </b-card>
    <b-modal
      v-model="show"
      title="Agregar Usuario"
      @hidden="editMode = false"
    >
      <b-container fluid>
        <b-row class="my-1">
          <b-col sm="2">
            <label for="input-small">Nombre:</label>
          </b-col>
          <b-col sm="10">
            <b-form-input id="input-small" size="sm" v-model="usuario_editable.nombre" ></b-form-input>
          </b-col>
        </b-row>
        <b-row class="my-1">
          <b-col sm="2">
            <label for="input-small">Correo:</label>
          </b-col>
          <b-col sm="10">
            <b-form-input id="input-small" size="sm" v-model="usuario_editable.correo_electronico" ></b-form-input>
          </b-col>
        </b-row>
        <b-row class="my-1">
          <b-col sm="2">
            <label for="input-small">Teléfono:</label>
          </b-col>
          <b-col sm="10">
            <b-form-input id="input-small" size="sm" v-model="usuario_editable.telefono" ></b-form-input>
          </b-col>
        </b-row>
        <b-row class="my-1">
          <b-col sm="2">
            <label for="input-small">Foto:</label>
          </b-col>
          <b-col sm="10">
            <b-form-input id="input-small" size="sm" v-model="usuario_editable.foto" ></b-form-input>
          </b-col>
        </b-row>
        <b-row class="my-1">
          <b-col sm="2">
            <label for="input-small">Clave:</label>
          </b-col>
          <b-col sm="10">
            <b-form-input id="input-small" size="sm" v-model="usuario_editable.clave" ></b-form-input>
          </b-col>
        </b-row>
      </b-container>
      <template v-slot:modal-footer>
        <div class="w-100" >
          <b-row align-h="center" >
            <b-button
              :variant="`${editMode ? 'primary' : 'success' }`"
              size="sm"
              class="mx-2"
              @click="guardarUsuario()"
            >
              Guardar
            </b-button>
            <b-button
              variant="danger"
              size="sm"
              class="mx-2"
              @click="show = false, editMode = false"
            >
              Cancelar
            </b-button>
          </b-row>
        </div>
      </template>
    </b-modal>
  </b-container>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data () {
    return {
      search: null,
      tablaVisible: true,
      nuevo_usuario: {
        id_usuario: 99,
        nombre: null,
        telefono: null,
        correo_electronico: null,
        clave: '1234',
        fecha_nacimiento: null,
        foto: 'https://i.ibb.co/Q9KFRjr/avatar.png'
      },
      usuario_editable: {},
      items: [
        { id_usuario: 2, nombre: 'Robert E. Miller', telefono: '701-884-7236', correo_electronico: 'RobertEMiller@gustr.com', clave: 'Fee3uemi0cee', fecha_nacimiento: '03/10/2020', foto: 'https://www.flaticon.es/svg/static/icons/svg/1154/1154462.svg' },
        { id_usuario: 1, nombre: 'Hung C. Ludwig', telefono: '817-522-9620', correo_electronico: 'HungCLudwig@superrito.com', clave: 'weik2Ohg', fecha_nacimiento: '03/10/2020', foto: 'https://i.ibb.co/Q9KFRjr/avatar.png' },
        { id_usuario: 3, nombre: 'Dina R. Wagner', telefono: '843-903-1659', correo_electronico: 'DinaRWagner@superrito.com', clave: 'OHohMiZ8ah', fecha_nacimiento: '03/10/2020', foto: 'https://www.flaticon.es/svg/static/icons/svg/1154/1154473.svg' },
        { id_usuario: 4, nombre: 'Danny D. Jose', telefono: '919-952-5560', correo_electronico: 'DannyDJose@superrito.com', clave: 'AhlieBei6', fecha_nacimiento: '03/10/2020', foto: 'https://www.flaticon.es/svg/static/icons/svg/1154/1154453.svg' }
      ],
      fields: [
        { label: 'Nombre Completo', key: 'nombre', sortable: true },
        { label: 'Correo Electrónico', key: 'correo_electronico', sortable: true },
        { label: 'Teléfono', key: 'telefono', sortable: true },
        { label: 'Clave', key: 'clave', sortable: true },
        { label: 'Foto', key: 'foto', sortable: true },
        { label: 'Acciones', key: '_edit', sortable: false }
      ],
      show: false,
      editMode: false
    }
  },
  methods: {
    editarUsuario (id_usuario) {
      this.usuario_editable = { ...this.items.filter(item => item.id_usuario === id_usuario)[0] }
      this.show = true
      this.editMode = true
    },
    eliminarUsuario (id_usuario) {
      let objItem = this.items.filter(item => item.id_usuario === id_usuario)
      let indexObj = this.items.indexOf(objItem[0])
      this.items.splice(indexObj, 1)
    },
    guardarUsuario () {
      if (this.editMode) {
        let objItem = this.items.filter(item => item.id_usuario === this.usuario_editable.id_usuario)
        let indexObj = this.items.indexOf(objItem[0])
        this.items.splice(indexObj, 1, { ...this.usuario_editable })
      } else {
        this.usuario_editable.id_usuario = this.items.length ? (this.items.length + 2) : 1
        this.items.push({ ...this.usuario_editable })
      }
      this.$forceUpdate()
      this.show = false
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
