<template>
  <div class="jumbotron">
    <h1 style="color: white; padding: 3%" class="display-4">Customers</h1>
    <div
      class="card"
      style="width: 25rem"
      v-for="(item, index) in dataPerUser"
      :key="index"
    >
      <img style="width: 60%; margin: 2%;" v-bind:src="item.avatar" />
      <h3 style="text-transform: uppercase">{{ item.user }}</h3>
      <p><b>Total bought: </b>{{ item.total_cost }}</p>
      <p><b>Amount paid: </b>{{ item.paid_amount }}</p>
      <p><b>Still owes: </b>{{ item.debt }}</p>
    </div>
    <div style="color:grey;margin-top:20%; padding-bottom:5%"><h1>JSON</h1>
    {{dataPerUser}}
    </div>
  </div>
</template>

<script>
export default {
  name: "Customers",
  data() {
    return {
      orders: [],
      payments: [],
      prices: [],
      dataPerUser: [],
      isFetched: { orders: false, payments: false, prices: false },
    };
  },
  async created() {
    this.fetchData();
  },
  methods: {
    isLoadedCheck() {
      if (
        this.isFetched.orders &&
        this.isFetched.prices &&
        this.isFetched.payments
      ) {
        this.getdataPerUser();
        this.calculateDebt();
      }
    },
    fetchData() {
      fetch("./data/orders.json")
        .then((response) => response.json())
        .then((data) => {
          this.orders = data;
          this.isFetched.orders = true;
          this.isLoadedCheck();
        });
      fetch("./data/payments.json")
        .then((response) => response.json())
        .then((data) => {
          this.payments = data;
          this.isFetched.payments = true;
          this.isLoadedCheck();
        });

      fetch("./data/prices.json")
        .then((response) => response.json())
        .then((data) => {
          this.prices = data;
          this.isFetched.prices = true;
          this.isLoadedCheck();
        });
    },

    //merges the data so that each occurs once 
    getdataPerUser() {
      var obj = this.orders.reduce(function (p, c) {
        var key = c.user;

        // if the object doesn't exist, create a new one
        // and set it's value as a pre-filled object
        p[key] = p[key] || { drink: [], size: [], number: key };

        // then add the variables to the title array
        p[key].drink.push(c.drink);
        p[key].size.push(c.size);

        return p;
      }, {});

      this.dataPerUser = Object.keys(obj).map(function (el) {
        return { drink: obj[el].drink, size: obj[el].size, user: el };
      });
    },
    calculateDebt() {
      //loop through users
      for (var j = 0; j < this.dataPerUser.length; j++) {
        var cost = 0;
        var paidAmount = this.payments.find(
          (payment) => payment.user == this.dataPerUser[j].user
        ).amount;

        //loop through orders of users and calculate total cost
        for (var i = 0; i < this.dataPerUser[j].drink.length; i++) {
          //find price of order
          var drink_price = this.prices.find(
            (price) => price.drink_name == this.dataPerUser[j].drink[i]
          );
          cost += drink_price.prices[this.dataPerUser[j].size[i]];
        }

        this.dataPerUser[j]["paid_amount"] = paidAmount;
        this.dataPerUser[j]["total_cost"] = cost;
        this.dataPerUser[j]["debt"] = cost - paidAmount;
        this.dataPerUser[j]["avatar"] = "https://avatars.dicebear.com/v2/avataaars/" + this.dataPerUser[j].user + ".svg";

        console.log(this.dataPerUser[j]);
      }
    },
  },
};
</script>

<style>
.card {
  display: inline-block;
  margin: 5px;
}

.jumbotron {
  background: rgb(7, 29, 5);
}
</style>