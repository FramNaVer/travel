<script setup>
import { ref, computed } from 'vue';


//variable drinking
const travelList = ref([
  { name:'Nan', price: 690, img: "https://mini-game.org/wp-content/uploads/2020/12/1440253430-IMG5082-o.jpg", quantity: 0 },
  { name:'Chiang Mai', price: 700, img: "https://res.klook.com/images/fl_lossy.progressive,q_65/c_fill,w_1295,h_862/w_80,x_15,y_15,g_south_west,l_Klook_water_br_trans_yhcmh3/activities/snciuzs3bodde5pdmqhs/%E0%B8%97%E0%B8%B1%E0%B8%A7%E0%B8%A3%E0%B9%8C%E0%B8%95%E0%B8%B1%E0%B8%A7%E0%B9%80%E0%B8%A1%E0%B8%B7%E0%B8%AD%E0%B8%87%E0%B9%81%E0%B8%A5%E0%B8%B0%E0%B8%A7%E0%B8%B1%E0%B8%94%E0%B9%83%E0%B8%99%E0%B8%88%E0%B8%B1%E0%B8%87%E0%B8%AB%E0%B8%A7%E0%B8%B1%E0%B8%94%E0%B9%80%E0%B8%8A%E0%B8%B5%E0%B8%A2%E0%B8%87%E0%B9%83%E0%B8%AB%E0%B8%A1%E0%B9%88%E0%B8%84%E0%B8%A3%E0%B8%B6%E0%B9%88%E0%B8%87%E0%B8%A7%E0%B8%B1%E0%B8%99.jpg", quantity: 0 },
  { name:'Lampang', price: 550, img: "https://paikondieow.com/wp-content/uploads/2019/06/13-1-2.jpg", quantity: 0 },
  { name:'Phuket', price: 2000, img: "https://travel.mthai.com/app/uploads/2018/03/Phuket2_re.jpg", quantity: 0 },
  { name:'Krabi', price: 1850, img: "https://www.doa.go.th/agrotour/wp-content/uploads/2019/02/i.jpg", quantity: 0 },
  { name:'Bangkok', price: 890, img: "https://www.ecocar.co.th/upload/1949122845.jpg", quantity: 0 },

])


//การจองตั๋ว
const list_local = ref([


]);


//function
function list_local_control(name, quantity) {
  const existingItemIndex = list_local.value.findIndex(item => item.name === name);

  if (existingItemIndex !== -1) {
    list_local.value[existingItemIndex].quantity += quantity;
    list_local.value[existingItemIndex].price = list_local.value[existingItemIndex].quantity * travelList.value.find(item => item.name === name).price;
  } else {
    const item = {
      name: name,
      quantity: quantity,
      price: quantity * travelList.value.find(item => item.name === name).price
    };
    list_local.value.push(item);
  }
}


function incrementQuantity(item) {
  item.quantity++;
  item.price = item.quantity * travelList.value.find((travel) => travel.name === item.name).price;
}

function decrementQuantity(item) {
  if (item.quantity > 0) {
    item.quantity--;
    item.price = item.quantity * travelList.value.find((travel) => travel.name === item.name).price;
  }
}

function removeItem(index) {
  list_local.value.splice(index, 1);
}



//pop up

const form_costumer = ref({
  name: "",
  phone: "",
  date: "",
  time: ""
});

const showPopup = ref(false);

const totalCost = computed(() => {
  return list_local.value.reduce((acc, item) => acc + item.price, 0);
});


function togglePopup() {
  showPopup.value = !showPopup.value;
}


</script>



<template>

  <HelloWorld v-if="showPopup" />
  <div class="overlay" v-if="showPopup">
    <HelloWorld />
  </div>

  <div class="header">
    <h2>Wherever you want to go</h2>
  </div>


  <div class="container text-center">
    <div class="row">
      <div class="col" v-for="(i, index) in travelList" :key="index">
        <div class="card" style="width: 18rem;">
          <img :src="i.img" class="card-img-top">
          <div class="card-body">
            <h3 class="card-title">{{ i.name }}</h3>
            <p>Price Ticket : {{ i.price }}</p>
            <p class="card-text">Number of Ticket
              <input type="number" class="cout_value " v-model="i.quantity">
            </p>
            <a class="btn btn-primary" @click="list_local_control(i.name, i.quantity)"> Confirm </a>
          </div>
        </div>
      </div>
    </div>
  </div>



  <h1 style="text-align: center; " v-if="list_local.length > 0" class="header">Reservation list</h1>
  <hr>
  <div class="list_cout text-center">
    <table class="table table-hover" v-if="list_local.length > 0">
      <table class="table">
        <thead>
          <tr>
            <th scope="col">Numer</th>
            <th scope="col">Title</th>
            <th scope="col">Number Ticket</th>
            <th scope="col">Price</th>
            <th scope="col"></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(i, index) in list_local" :key="index">
            <th scope="row">{{ index + 1 }}</th>
            <td>{{ i.name }}</td>
            <td>
              <div class="quantity-controls">
                <button type="button" class="btn btn-danger" @click="decrementQuantity(i)" :disabled="i.quantity === 0"> -
                </button>
                <span>{{ i.quantity }}</span>
                <button type="button" class="btn btn-success" @click="incrementQuantity(i)">+</button>
              </div>
            </td>
            <td>{{ i.price }}</td>
            <td>
              <button type="button" class="btn btn-danger" @click="removeItem(index)">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
    </table>
  </div>


  <div class="contrainer_costumer " v-if="list_local.length > 0">
    <div class="content-infomation">
      <div class="form_box">
      <h2 style="text-align: center;">Infomation</h2>
      <hr>
    </div>
    <form>
      <div class="form_box">
        <label for="name"></label><br>
        <input type="text" id="name" class="form-control" v-model="form_costumer.name" placeholder="Fist/last Name" required>
      </div>
      <div class="form_box">
        <label for="phone"></label><br>
        <input type="tel" id="phone" class="form-control" v-model="form_costumer.phone" placeholder="Number Phone" required>
      </div>
      <div class="form_box">
        <label for="date"></label><br>
        <input type="date" id="date" class="form-control" v-model="form_costumer.date" placeholder="Day" required>
      </div>
      <div class="form_box">
        <label for="time"></label><br>
        <input type="time" id="time" class="form-control" v-model="form_costumer.time" placeholder="Datetime" required>
      </div>
      <div class="form_box">
        <button type="button" class="btn btn-success" @click="togglePopup"> Confirm Ticket </button>
      </div>
    </form>
    </div>
  </div>


  <div v-if="showPopup" class="overlay">

    <div class="popup">
      <h3 style="text-align: center; color:green ;">Successfully</h3>
      <p><strong>First/last Name</strong> {{ form_costumer.name }}</p>
      <p><strong>Number Phone</strong> {{ form_costumer.phone }}</p>
      <p><strong>Day</strong> {{ form_costumer.date }}</p>
      <p><strong>Datetime</strong> {{ form_costumer.time }}</p>

      <table class="box_table" style="text-align: center;">
        <thead>
          <tr>
            <th>Number</th>
            <th>Title</th>
            <th>Number Ticket</th>
            <th>Price</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(i, index) in list_local" :key="index" style="text-align: center;">
            <td>{{ index + 1 }}</td>
            <td>{{ i.name }}</td>
            <td>{{ i.quantity }}</td>
            <td>{{ i.price }} Bath </td>
          </tr>
        </tbody>
      </table>

      <p><strong>Total:</strong> {{ totalCost }} Bath</p>

      <button class="btn btn-danger" @click="showPopup = false">Close Window</button>
    </div>
  </div>
</template>


<style>
* {
  padding: 5px;
}

.header {
  text-align: center;
}

.card-img-top {
  height: 100%;
  padding: 25px;
}

.col {
  padding: 10px;
}


.contrainer_customer {
  justify-content: center;
  align-items: center;
  margin-left: 20%;
  margin-right: 20%;

}

.content-infomation {
  border: 2px solid #7c7c7c;
  border-radius: 15px;
}

.form_box {
  justify-content: center;
  align-items: center;
  padding: 5px;
  margin-left: 30%;
  margin-right: 30%;
}


.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
}


.popup {
  background-color: #fff;
  padding: 20px;
  border-radius: 10px;
  width: 70%;
  max-height: 80%;
  overflow-y: auto;
}

.header {
  padding: 25px;
}

.card-icon img {
  height: 125px;
  padding: 10px;
  margin-left: 430px;
}
</style>