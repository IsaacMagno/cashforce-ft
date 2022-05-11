<template>
  <div class="container-fluid mt-5" id="root">
    <div class="row">
      <div class="col-2 mb-5">
        <img :src="getImgUrl('logo-cashforce.png')" alt="Cashforce logo" />
      </div>
    </div>
    <div class="row">
      <div class="col-2">
        <button
          id="btn-invoice"
          v-on:mouseover="icon = handWhite"
          v-on:mouseout="icon = handGreen"
          type="button"
          v-on:click="showComponent = !showComponent"
        >
          <img :src="icon" id="handGreen" />
          Notas fiscais
        </button>
      </div>
      <div class="col">
        <div class="container card p-5" v-if="showComponent">
          <OrdersComponent />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import OrdersComponent from "./OrdersComponent.vue";

export default {
  components: {
    OrdersComponent,
  },
  data() {
    return {
      handGreen: this.getImgUrl("handgreen.png"),
      handWhite: this.getImgUrl("handwhite.png"),
      icon: this.getImgUrl("handgreen.png"),
      showComponent: true,
    };
  },
  methods: {
    getImgUrl(pic) {
      return require("../assets/" + pic);
    },
  },
};
</script>

<style>
body {
  background-color: #fbfdfe;
}

#btn-invoice {
  border-radius: 12px;
  width: 200px;
  height: 45px;
  font-size: 15px;
  cursor: pointer;
  border: none;
  outline: none;
  background: transparent;
  color: #00ad8c;
  position: relative;
  transition: all 0.3s;
  z-index: 1;
}

#btn-invoice::before {
  border-radius: 12px;
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 5px;
  height: 100%;
  background-color: #00ad8c;
  z-index: -1;
  transition: all 0.3s;
}

#btn-invoice:hover::before {
  width: 100%;
}

#btn-invoice:hover {
  color: #ffffff;
}

#btn-invoice:active:before {
  background: #b9b9b9;
}

#handGreen {
  width: 20px;
  margin-right: 15px;
}
</style>
