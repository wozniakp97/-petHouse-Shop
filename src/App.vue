<template>
  <div class="general_container">
    <h1>Lista produktów</h1>
    <div class="button_container">
      <button class="button_primary" :style="'margin-right: 20px'" @click="isModalVisibleCategory = true">Dodaj nową
        kategorię</button>
      <button class="button_primary" @click="isModalVisible = true">Dodaj nowy produkt</button>
    </div>
    <div class="table_container">
      <table>
        <thead>
          <tr>
            <th colspan="1">Id</th>
            <th colspan="1">
              <div class="container_title">
                <div class="arrow_up" @click="sortsAlphabeticallyName"></div>
                <p>Kategoria</p>
                <div class="arrow_down" @click="sortsFromDehindName"></div>
              </div>
            </th>
            <th colspan="1">
              <div class="container_title">
                <div class="arrow_up" @click="sortsAlphabeticallyType"></div>
                <p>Dla jakiego zwierzęcia</p>
                <div class="arrow_down" @click="sortsFromDehindType"></div>
              </div>
            </th>
            <th colspan="1">
              <div class="container_title">
                <div class="arrow_up" @click="sortsAlphabeticallyPrice"></div>
                <p>Cena</p>
                <div class="arrow_down" @click="sortsFromDehindPrice"></div>
              </div>
            </th>
            <th>Akcja</th>
          </tr>
          <tr>
            <th></th>
            <th>
              <input placeholder="Szukaj" class="input_search" @keyup="(e) => searchName(e.target.value)">
            </th>
            <th>
              <input placeholder="Szukaj" class="input_search" @keyup="(e) => searchType(e.target.value)">
            </th>
            <th>
              <input placeholder="Szukaj" class="input_search" @keyup="(e) => searchPrice(e.target.value)">
            </th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(el, index) in workerTable" :key="index">
            <td>{{ el.id }}</td>
            <td>{{ el.name }}</td>
            <td>{{ el.type }}</td>
            <td>{{ parseFloat(el.price).toFixed(2) }} {{ format }}</td>
            <td>
              <div class="delete" :id="el.id" @click="(e) => deletes(e)"></div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div>
      <button class="button_primary" @click="formaterEUR" v-if="format == 'PLN'">Przelicz na euro</button>
      <button class="button_primary" @click="formaterPLN" v-if="format == 'EUR'">Przelicz na złotówki</button>
    </div>
    <teleport to="#modal">
      <ModalNewProduct v-show="isModalVisible" v-model:zmienna="isModalVisible" :table="generalTable"
        :categoryTable="categoryTable" @custom-change="handleCustomChange"></ModalNewProduct>
    </teleport>
    <teleport to="#modalCategory">
      <ModalNewCategory v-show="isModalVisibleCategory" v-model:category="isModalVisibleCategory"
        :categoryTable="categoryTable" @custom-change="handleChangeCategory"></ModalNewCategory>
    </teleport>
  </div>
</template>

<script>
import ModalNewCategory from './components/ModalNewCategory.vue'
import ModalNewProduct from './components/ModalNewProduct.vue'

import { ref } from 'vue'

export default {
  name: 'App',
  components: {
    ModalNewProduct,
    ModalNewCategory
  },
  mounted() {
    this.workerTable = this.generalTable;
  },
  data() {
    return {
      format: 'PLN',
      workerTable: [],
      categoryTable: [
        {
          id: 1,
          name: 'Na drzewie',
        },
        {
          id: 2,
          name: 'Na ziemi',
        }
      ],
      generalTable: [
        {
          id: 1,
          name: 'Na drzewie',
          type: 'Pies',
          price: '250'
        }
      ]
    }
  },


  methods: {

    deletes(e) {
      this.workerTable = this.workerTable.filter(el => el.id != e.target.id)
      this.generalTable = this.generalTable.filter(el => el.id != e.target.id)


    },
    formaterEUR() {
      this.format = 'EUR',
        this.workerTable.forEach((el) => {
          el.price = parseFloat(parseFloat(el.price) / 4.69).toFixed(2)
        })
    },
    formaterPLN() {
      this.format = 'PLN',
        this.workerTable.forEach((el) => {
          el.price = parseFloat(parseFloat(el.price) * 4.69).toFixed(2)
        })
    },
    handleCustomChange(newProduct) {
      this.uppercase = newProduct
      this.generalTable.push({
        id: this.generalTable.length + 1,
        name: newProduct.name,
        type: newProduct.type,
        price: parseFloat(newProduct.price)
      })
      this.workerTable = this.generalTable;
    },

    handleChangeCategory(s) {
      this.uppercase = s
      this.categoryTable.push({
        id: this.categoryTable.length + 1,
        name: s.name,
      })
    },

    searchName(e) {
      const searchWord = e.toUpperCase();
      this.workerTable = this.generalTable.filter((worker) =>
        worker.name.toUpperCase().indexOf(searchWord) > -1
      )

    },
    searchType(e) {
      const searchWord = e.toUpperCase();
      this.workerTable = this.generalTable.filter((worker) =>
        worker.type.toUpperCase().indexOf(searchWord) > -1
      )

    },
    searchPrice(e) {
      const searchWord = e.toUpperCase();
      this.workerTable = this.generalTable.filter((worker) =>
        worker.price.toUpperCase().indexOf(searchWord) > -1
      )
    },

    sortsAlphabeticallyName() {
      this.workerTable.sort((a, b) => b.name.localeCompare(a.name))
    },
    sortsFromDehindName() {
      this.workerTable.sort((a, b) => a.name.localeCompare(b.name))
    },
    sortsAlphabeticallyType() {
      this.workerTable.sort((a, b) => b.type.localeCompare(a.type))
    },
    sortsFromDehindType() {
      this.workerTable.sort((a, b) => a.type.localeCompare(b.type))
    },
    sortsAlphabeticallyPrice() {
      this.workerTable.sort((a, b) => a.price.localeCompare(b.price))
    },
    sortsFromDehindPrice() {
      this.workerTable.sort((a, b) => b.price.localeCompare(a.price))
    }
  },

  setup() {
    const isModalVisible = ref(false);
    const isModalVisibleCategory = ref(false);
    return { isModalVisible, isModalVisibleCategory };
  }
}
</script>

<style lang="scss">
h1 {
  font-size: 30px;
}

.delete {
  background-image: url('./assets/delete.png');
  cursor: pointer;
  background-repeat: no-repeat;
  background-size: contain;
  width: 30px;
  height: 30px;
}

.input_search {
  font-size: 14px;
  box-sizing: border-box;
  width: 100%;
  height: 37px;
  padding: 4px 8px 4px 8px;
  border: 1px solid #000;
  background: #fcfcfc;
  border-radius: 5px;
  vertical-align: top;
  color: #161616;
}

.container_title {
  display: flex;
  align-items: center;
  justify-content: center;
}

.arrow_up {
  width: 0;
  height: 0;
  cursor: pointer;
  margin: 10px;
  border-left: 10px solid transparent;
  border-right: 10px solid transparent;
  border-bottom: 10px solid black;
}

.arrow_down {
  width: 0;
  height: 0;
  cursor: pointer;
  margin: 10px;
  border-top: 10px solid black;
  border-left: 10px solid transparent;
  border-right: 10px solid transparent;
}


.button_container {
  display: flex;
  justify-content: flex-end;
  margin: 15px;
}

.button_primary {
  background-color: #555555;
  cursor: pointer;
  border-radius: 10px;
  border: none;
  color: white;
  padding: 15px 15px;
  text-align: center;
  width: 14rem;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  border: 1px solid #555555;

  &:hover {
    background-color: white;
    color: #555555;
    border: 1px solid #555555;
  }
}


.table_container {
  display: flex;
  justify-content: center;
  margin: 15px;
}

.general_container {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
}

tr:nth-child(2n) {
  background-color: rgba(128, 128, 128, 0.445);
}

tr:nth-child(2n+1) {
  background-color: rgba(128, 128, 128, 0.677);
}

table,
td {
  text-align: -webkit-center;
}

table {
  width: 100%;
  font-size: 16px;
}

thead,
tfoot {
  color: #fff;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>