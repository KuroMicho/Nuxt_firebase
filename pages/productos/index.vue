<template>
  <div>
    <div class="row mt-3">
      <div class="col-xs-6">
        <h4>Listado de Productos</h4>
      </div>
      <div class="col-xs-6 ml-2">
        <b-button variant="primary" href="/productos/nuevo">Nuevo</b-button>
      </div>
    </div>

    <div class="row mt-2">
      <div class="col-sm-12">
        <b-table
          responsive
          striped
          hover
          :fields="fields"
          :items="items"
          :current-page="currentPage"
          :per-page="perPage"
          id="Productos"
        >
          <template slot="Acciones">
            <b-button variant="success">Editar</b-button>
            <b-button variant="danger">Eliminar</b-button>
          </template>
        </b-table>
      </div>
    </div>

    <b-pagination
      v-model="currentPage"
      :total-rows="rows"
      :per-page="perPage"
      aria-controls="Productos"
    ></b-pagination>
  </div>
</template>

<script>
import firebase, { db } from "../../services/firebase";

export default {
  asyncData() {
    let perPage = 3;
    return (
      db
        .collection("Productos")
        .orderBy("Nombre")
        //.limit(perPage)
        .get()
        .then(async res => {
          let items = [];
          res.forEach(value => {
            items.push(value.data());
          });
          //let resTotal = await db.collection("Productos").get();
          return {
            items,
            currentPage: 1,
            rows: res.docs.length,
            perPage
          };
        })
    );
  },
  data() {
    return {
      fields: [
        "Imagen",
        { key: "Nombre", sortable: true },
        { key: "Precio", sortable: true },
        { key: "Cantidad", sortable: true },
        { key: "Acciones", sortable: false }
      ]
    };
  }
};
</script>

