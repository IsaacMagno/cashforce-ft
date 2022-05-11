<template>
  <div>
    <div class="row mb-3">
      <div class="col-4">
        <h3 class="fw-bold">
          <img :src="getImgUrl('blueHands.png')" id="blueHands" />
          Notas Fiscais
        </h3>
        <p id="p-head">Visualize as notas fiscais que você tem.</p>
      </div>
      <div v-if="showDetails" class="col">
        <ProviderComponent :detail="this.provData" />
      </div>
    </div>
    <div class="row">
      <table class="table-sm">
        <thead>
          <tr class="spaceUnder">
            <th scope="col">NOTA FISCAL</th>
            <th scope="col">SACADO</th>
            <th scope="col">CEDENTE</th>
            <th scope="col">EMISSÃO</th>
            <th scope="col">VALOR</th>
            <th scope="col">STATUS</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(d, i) in allOrd" v-bind:key="i" class="spaceUnder">
            <td scope="row">{{ d.orderNfId }}</td>
            <td>{{ d.buyerId }}</td>
            <td>{{ d.providerId }}</td>
            <td>{{ d.emissionDate }}</td>
            <td class="ordVal fw-bold">{{ d.value }}</td>
            <td class="ordVal fw-bold">{{ d.orderStatusBuyer }}</td>
            <td>
              <button
                type="button"
                class="btn btn-outline-secondary btn-sm"
                id="provider-data"
                v-on:click="this.showDetails = !showDetails"
              >
                Dados do Cedente
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";

import ProviderComponent from "./ProviderComponent.vue";

const URL = "https://cashforce-db.herokuapp.com";

export default {
  components: {
    ProviderComponent,
  },
  data() {
    return {
      ordData: [],
      orderStatus: [
        "Pendente de confirmação",
        "Pedido confirmado",
        "Não reconhece o pedido",
        "Mercadoria não recebida",
        "Recebida com avaria",
        "Devolvida",
        "Recebida com devolução parcial",
        "Recebida e confirmada",
        "Pagamento Autorizado",
      ],
      buyData: [],
      provData: [],
      showDetails: false,
    };
  },
  async created() {
    const getOrders = await axios.get(`${URL}/orders`);
    this.ordData = getOrders.data;
    const getBuyers = await axios.get(`${URL}/buyers/`);
    this.buyData = getBuyers.data;
    const getProviders = await axios.get(`${URL}/providers/`);
    this.provData = getProviders.data;
  },
  methods: {
    format_date(value) {
      if (value) {
        const newData = moment(String(value)).format("DD/MM/YYYY");
        return newData;
      }
    },

    find_buyer(id) {
      return this.buyData.filter((buyer) => buyer.id === id)[0].name;
    },

    find_provider(id) {
      return this.provData.filter((provider) => provider.id === id)[0].name;
    },

    getImgUrl(pic) {
      return require("../assets/" + pic);
    },
  },
  computed: {
    allOrd() {
      return this.ordData.map((ord) => ({
        ...ord,
        emissionDate: this.format_date(ord.emissionDate),
        value: parseInt(ord.value).toLocaleString("pt-br", {
          style: "currency",
          currency: "BRL",
        }),
        orderStatusBuyer: this.orderStatus[parseInt(ord.orderStatusBuyer)],
        buyerId: this.find_buyer(ord.buyerId),
        providerId: this.find_provider(ord.providerId),
      }));
    },
  },
};
</script>

<style>
h3 {
  color: #032059;
}

th {
  color: #a1a8b8;
  font-size: 13px;
}

td {
  color: #4d5566;
  font-size: 14px;
}

tr.spaceUnder > th {
  padding-bottom: 1em;
}

tr.spaceUnder > td {
  padding-bottom: 1em;
}

#p-head {
  color: #727d94;
  font-size: 15px;
}

#blueHands {
  width: 33px;
  margin-right: 10px;
}

#provider-data {
  border-radius: 16px;
  padding-left: 25px;
  padding-right: 25px;
}

.ordVal {
  color: #00ad8c;
}
</style>
