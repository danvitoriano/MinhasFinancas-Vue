<template>
  <div>
    <div class="align-center margins">
      <button type="button" id="btn-import" v-on:click="importFinances">Importar</button>
    </div>

    <form class="form">
      <label for="item">Item:</label>
      <input id="item" v-model="item" type="text" placeholder="Item" />

      <label for="date">Data:</label>
      <input id="date" v-model="date" type="date" placeholder="dd/mm/aaaa" />

      <label for="quantity">Quantidade:</label>
      <input id="quantity" v-model="quantity" type="number" placeholder="00" />

      <label for="value">Valor:</label>
      <input id="value" v-model="value" type="number" placeholder="00.00" />

      <div class="align-center margins">
        <button type="button" class="btn" id="btn-submit" v-on:click="onSubmit">Incluir</button>
      </div>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "RegisterFinance",
  data() {
    return {
      item: null,
      date: null,
      quantity: null,
      value: null,
      errors: []
    };
  },
  methods: {
    onSubmit() {
      this.errors = [];
      if (this.item && this.date && this.quantity && this.value) {
        let finance = {
          item: this.item,
          date: this.date,
          quantity: this.quantity,
          value: this.value
        };
        this.$emit("create-finance", finance);
        (this.item = null),
          (this.date = null),
          (this.quantity = null),
          (this.value = null);
      } else {
        if (!this.item) this.errors.push("Item obrigatório");
        if (!this.date) this.errors.push("Data obrigatório");
        if (!this.quantity) this.errors.push("Quantidade obrigatório");
        if (!this.value) this.errors.push("Valor obrigatório");
      }
    },
    importFinances() {
      let finances;
      axios
        .get("https://evening-badlands-20922.herokuapp.com/financas/semana")
        .then(response => {
          finances = response.data;
          this.$emit("import-finances", finances);
        })
        .catch(e => {
          this.erros.push(e);
        });
    }
  }
};
</script>

<style scoped>
.body {
  width: 40%;
  margin: 0px auto;
  font-family: tahoma;
  color: #282828;
}

.align-center {
  text-align: center;
}

label {
  font-size: 16px;
}

.logo {
  width: 72px;
  height: 72px;
  margin: 0px auto;
}

.color-box {
  width: 40px;
  height: 40px;
  margin-top: 5px;
}

button {
  margin-top: 30px;
  border: none;
  background-color: #1e95ea;
  color: white;
  height: 40px;
  width: 100px;
  font-size: 14px;
}

.form {
  width: 100%;
  padding: 20px;
  border: 1px solid #d8d8d8;
}

input {
  width: 96%;
  height: 25px;
  margin-bottom: 20px;
  padding: 5px;
  border-radius: 5px;
  box-shadow: inset 0px 0px 0px 0px;
  font-size: 14px;
  margin-top: 10px;
}
.btn {
  margin: 0px auto;
}

.margins {
  margin: 20px 0px;
}

.table {
    width: 100%
}

.width {
    text-align: left;
    width: 25%
}
</style>
