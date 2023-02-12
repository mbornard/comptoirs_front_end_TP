<template>
  <main>
    <div>
      <p class = "pages">Les produits - page {{data.page}} / {{data.totalPages}}</p>
      <table>
        <caption>Liste des catégories</caption>
        <tr>
          <th>Nom</th>
          <th>Prix</th>
          <th>Stock</th>
          <th>Commandé</th>
        </tr>
        <!-- Si le tableau des catégories n'est pas vide -->
        <tr v-for="produit in data.listeProduits" :key="produit.reference">
          <td>{{ produit.nom }}</td>
          <td>{{ produit.prixUnitaire }}</td>
          <td>{{ produit.unitesEnStock }}</td>
          <td>{{ produit.unitesCommandees }}</td>
        </tr>
      </table>
      <button @click="data.page = 0; chargeProduit()">Page 1</button>
      <button @click="data.page = data.page - 1; chargeProduit()" :disabled="data.page === 0"> &lt- </button>
      <button @click="data.page = data.page + 1; chargeProduit()" :disabled="data.page=== data.totalPages">-></button>
      <button @click="data.page = data.totalPages; chargeProduit()">Dernière page</button>

    </div>
  </main>
</template>

<script setup>
import { reactive, onMounted } from "vue";
import { doAjaxRequest } from "@/api";

let data = reactive({
  size:5,
  page:0,
  totalPages:0,
  listeProduits: []
});

function chargeProduit() {
  // Appel à l'API pour avoir la liste des catégories
  // Trié par code, descendant
  // Verbe HTTP GET par défaut
  doAjaxRequest("https://springajax.herokuapp.com/api/produits?page="+ data.page + "&size="+data.size)
      .then((json) => {
        data.listeProduits = json._embedded.produits;
        data.totalPages = json.page.totalPages-1;

      })
      .catch((error) => alert(error.message));
}

// A l'affichage du composant, on affiche la liste
onMounted(chargeProduit);

</script>


<style scoped>
td,
th {
  border: 1px solid #ddd;
  padding: 8px;
}


th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #232623;
  color: rgb(255, 255, 255);
}
button{
  padding : 10px;
  margin-left: 40px;
}
table{
  width: 70%;
}
.pages
{
  text-indent: 100px;
  font-size: large;

}


</style>