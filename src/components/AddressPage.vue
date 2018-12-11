<style scoped lang="less">
@import "../less/common.less";

.address-page {
  background-color: #dfdfdf;
  .navbar-fixed-top {
    background-color: #262627;
  }
  display: flex;
  flex-direction: column;
  min-height: 100%;
}

.address-body {
  flex: 1;
  width: 100%;
  background-size: cover;
  background-position: bottom center;
  color: var(--primary-text-color);
  display: flex;
  padding-top: 6em;

  .tx-hash {
    margin: @space-md 0;
  }

  .tx-table {
    border-collapse: collapse;
    width: 100%;
    text-align: left;
    margin-bottom: 3em;
    td.td-title {
      font-weight: bold;
    }
    td {
      padding: @space-sm;
    }
  }
}
</style>

<template>
  <div class="address-page page">
    <header class="navbar-fixed-top">
      <div class="container">
        <div class="navbar-header">
          <router-link class="navbar-brand" to="/"></router-link>
        </div>
      </div>
    </header>

    <div class="address-body">
      <div class="container" v-if="address">
        <h1>Address</h1>
        <div class="tx-hash">
          <b>address Hash</b>
          {{ address.hash }}
        </div>

        <h2>Summary</h2>
        <table class="tx-table">
          <tr>
            <td class="td-title">Balance</td>
            <td>{{ address.balance }}</td>
          </tr>
          <tr>
            <td class="td-title">Transactions</td>
            <td>{{ address.txCount }}</td>
          </tr>
        </table>

        <h2>Transactions</h2>
        <table class="transactions-table">
          <tr>
            <th>TxHash</th>
            <th>Age</th>
            <th>From</th>
            <th>To</th>
            <th>Value</th>
          </tr>
          <tr v-for="tx in address.txs" :key="tx.id">
            <td>
              <router-link :to="'/tx/' + tx.id">{{ tx.id | shorten }}</router-link>
            </td>
            <td>{{ tx.timestamp }}</td>
            <td>
              <router-link :to="'/address/' + tx.from">{{ tx.from | shorten }}</router-link>
            </td>
            <td>
              <router-link :to="'/address/' + tx.to">{{ tx.to | shorten }}</router-link>
            </td>
            <td>{{ tx.value }}</td>
          </tr>
        </table>
      </div>
    </div>
    <site-footer></site-footer>
  </div>
</template>

<script>
import FontAwesomeIcon from "@fortawesome/vue-fontawesome";
import store from "../store";
import service from "../service";
import SiteFooter from "./SiteFooter";

export default {
  name: "AddressPage",
  data() {
    return {
      address: null
    };
  },
  components: {
    FontAwesomeIcon,
    SiteFooter
  },
  mounted() {
    service
      .getAddress(this.$route.params.address)
      .then(address => (this.address = address));
  }
};
</script>