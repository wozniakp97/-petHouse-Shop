<template>
  <div class="modal">
    <article class="modal__content">
      <section class="modal__text">
        <h1>Dodaj nową kategorię</h1>
      </section>
      <section class="modal__input_container">
        <label>Nazwa kategori</label>
        <input class="input_class" v-model="name">
      </section>
      <section class="modal__buttons">
        <button class="button_accept" @click="handleChangeCategory">Dodaj kategorię</button>
        <button class="button_cancel" @click="cancel">Anuluj</button>
      </section>
    </article>
    <teleport to="#modalAlert">
      <AlertModal v-show="isModalAlertActiveCategory" :msg="msg"></AlertModal>
    </teleport>
  </div>
</template>

<script>
import AlertModal from './AlertModal.vue';
import {ref} from 'vue'
export default {
  name: 'ModalNewProduct',
  components:{
    AlertModal
  },
  props: {
    category: {
      type: Boolean,
      default: true
    },
    categoryTable: {
      type: Array
    }
  },
  data() {
    return {
      name: '',
      msg: '',
    }
  },
  methods: {

    cancel() {
      this.name = '';
      this.$emit('update:category', false);
    },

    handleChangeCategory() {
      if (!this.name) {
        this.isModalAlertActiveCategory = true
        this.msg = 'Wpisz nazwę kategori.'
        setTimeout(() => {
          this.isModalAlertActiveCategory = false
        }, 3000);
      }
      else if (this.categoryTable.find(el => el.name == this.name)) {
        this.isModalAlertActiveCategory = true
        this.msg = 'Podana nazwa kategori istnieje. Spróbuj użyć innej nazwy.'
        setTimeout(() => {
          this.isModalAlertActiveCategory = false
        }, 3000);
      }
      else {
        this.$emit("customChange", { name: this.name });
        this.name = '';
        this.$emit('update:category', false);
      }
    }
  },
  setup(){
    const isModalAlertActiveCategory = ref(false)
    return{isModalAlertActiveCategory}
  }
}

</script>
   
<style lang="scss" scoped>
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
    width: 60%;
    height: 65%;
    border-radius: 10px;
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