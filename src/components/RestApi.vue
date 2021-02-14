<template>
  <v-container mt-9>
    <v-row>
      <v-col>
        <v-text-field
          v-model="endpoint"
          filled
          name="endpoint"
          placeholder="Endpoint"
          id="endpoint"
          prefix="https://crudcrud.com/api/"
          ><template v-slot:prepend>
            <v-tooltip bottom>
              <template v-slot:activator="{ on }">
                <v-icon v-on="on">
                  mdi-help-circle-outline
                </v-icon>
              </template>
              Der Endpoint wird beim betreten der CrudCrud Webseite erstellt und
              angezeigt. <br />
              Der generierte Endpoint und die Daten sind nur für 24h verfügbar.
            </v-tooltip>
          </template>
        </v-text-field>
      </v-col>
    </v-row>
    <v-row v-if="endpoint != ''">
      <crud-table :apiUrl="apiUrl" />
    </v-row>
  </v-container>
</template>

<script>
import CrudTable from "./CrudTable.vue";

export default {
  name: "RestApi",

  components: {
    CrudTable,
  },

  data: () => ({
    endpoint: "",
    hasConnection: false,
  }),

  computed: {
    activeColor: function() {
      return this.hasConnection ? "" : "success";
    },
    apiUrl: function() {
      return "https://crudcrud.com/api/" + this.endpoint;
    },
  },
};
</script>
