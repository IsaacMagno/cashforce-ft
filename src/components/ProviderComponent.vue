<template>
  <div class="card p-3" id="details">
    <ul
      v-for="(z, i) in renderDetails"
      v-bind:key="i"
      class="list-group list-group-flush"
    >
      <li class="list-group-item text-secondary">Nome: {{ z.name }}</li>
      <li class="list-group-item text-secondary">
        Nome comercial: {{ z.tradingName }}
      </li>
      <li class="list-group-item text-secondary">
        Criado em {{ z.createdAt }}
      </li>
      <li class="list-group-item text-secondary">
        Atualizado em {{ z.updatedAt }}
      </li>
    </ul>
  </div>
</template>

<script>
import moment from "moment";

moment.locale("pt-br");

export default {
  props: ["detail"],
  computed: {
    renderDetails() {
      return this.detail.map((d) => ({
        name: d.name,
        tradingName: d.tradingName,
        createdAt: moment(String(d.createdAt)).format("LLL"),
        updatedAt: moment(String(d.updatedAt)).format("LLL"),
      }));
    },
  },
};
</script>

<style>
#details {
  position: absolute;
}
</style>
