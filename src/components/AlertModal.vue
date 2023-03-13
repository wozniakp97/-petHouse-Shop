<template>
    <div class="modal">
      <article class="modal__content">
        <section class="modal__container">
          <h1>Uwaga!</h1>
          <p>{{msg}}</p>
        </section>
      </article>
    </div>
  </template>
  
  <script>
  export default {
    name: 'AlertModal',
    props:{
        msg:{
            type: String,
            default: ''
        }
    }
  }
  
  </script>
     
  <style lang="scss" scoped>

  .modal__container{
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 10px;
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
    z-index: 3;
  
    &__content {
      width: 30%;
      height: 40%;
      border-radius: 10px;
      background-color: #fff;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: space-around;
    }
  }
  </style>