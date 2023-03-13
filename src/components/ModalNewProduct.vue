<template>
  <div class="modal">
    <article class="modal__content">
      <section class="modal__text">
        <h1>Dodaj nowy produkt</h1>
      </section>
      <section class="modal__input_container">
        <label>Kategoria</label>
        <select v-model="selected" class="input_class" value="name">
          <option v-for="option in categoryTable" :key="option.id">{{ option.name }}</option>
        </select>
        <label>Dla jakiego zwierzęcia</label>
        <input class="input_class" v-model="type">
        <label>Cena</label>
        <input placeholder="PLN" type="number" class="input_class" v-model="price">
      </section>
      <section class="modal__buttons">
        <button class="button_accept" @click="handleChange">Dodaj produkt</button>
        <button class="button_cancel" @click="cancel">Anuluj</button>
      </section>
    </article>
    <teleport to="#modalAlert">
      <AlertModal v-show="isModalAlertActive" :msg="'Upewnij się, że uzupełniłeś poprawnie wszystkie pola.'"></AlertModal>
    </teleport>
  </div>
</template>
 
<script>
import {ref} from 'vue'
import AlertModal from './AlertModal.vue';
export default {
  name: 'ModalNewProduct',
  components: {
    AlertModal
  },
  props: {
    zmienna: {
      type: Boolean,
      default: true
    },
    table: {
      type: Array
    },
    categoryTable: {
      type: Array
    }
  },
  data() {
    return {
      tableSelect: [],
      type: '',
      price: '',
      selected: ''
    }
  },
  methods: {
    cancel() {
      this.price = '';
      this.type = '';
      this.selected = '';
      this.$emit('update:zmienna', false);
    },
    handleChange() {
      if (!this.price || !this.type || !this.selected) {
        this.isModalAlertActive = true
        setTimeout(() => {
          this.isModalAlertActive = false
        }, 3000);
      }
      else {
        this.$emit("customChange", { price: this.price, type: this.type, name: this.selected });
        this.price = '';
        this.type = '';
        this.selected = '';
        this.$emit('update:zmienna', false);
      }
    }
  },
  setup(){
        const isModalAlertActive = ref(false)

        return{isModalAlertActive}
    }
}

</script>
 
<style lang="scss">
.input_class {
  font-size: 16px;
  box-sizing: border-box;
  width: 100%;
  height: 38px;
  padding: 4px 8px 4px 8px;
  border: 1px solid #000;
  background: #fcfcfc;
  border-radius: 5px;
  vertical-align: top;
  color: #161616;
}

.button_cancel {
  background-color: #ff0000bf;
  border-radius: 10px;
  cursor: pointer;
  border: none;
  color: white;
  padding: 15px 15px;
  text-align: center;
  width: 14rem;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  border: 1px solid #ff0000bf;

  &:hover {
    background-color: white;
    color: #ff0000bf;
    border: 1px solid #ff0000bf;
  }
}

.button_accept {
  background-color: #0c8111;
  border-radius: 10px;
  cursor: pointer;
  border: none;
  color: white;
  padding: 15px 15px;
  text-align: center;
  width: 14rem;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  border: 1px solid #0c8111;

  &:hover {
    background-color: white;
    color: #0c8111;
    border: 1px solid #0c8111;
  }
}

.modal__input_container {
  display: flex;
  flex-direction: column;
  width: 70%;
  flex-direction: column;
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(0, 0, 0, 0.8);
  z-index: 2;

  &__content {
    width: 70%;
    border-radius: 10px;
    height: 75%;
    background-color: #fff;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-around;
  }

  &__buttons {
    width: 100%;
    display: flex;
    justify-content: space-around;
  }
}
</style>