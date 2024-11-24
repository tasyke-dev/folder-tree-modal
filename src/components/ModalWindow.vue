<template>
    <div class="modal-overlay" @click.self="close">
      <div class="modal">
        <header>
          <h2>{{ title }}</h2>
        </header>
        <section>
          <slot />
        </section>
        <footer>
          <button @click="close">Закрыть</button>
          <button @click="confirm">Ок</button>
        </footer>
      </div>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent } from 'vue';
  
  export default defineComponent({
    name: 'ModalWindow',
    props: {
      title: {
        type: String,
        required: true,
      },
    },
    emits: ['close', 'confirm'],
    setup(_, { emit }) {
      const close = () => {
        emit('close');
      };
  
      const confirm = () => {
        emit('confirm');
      };
  
      return { close, confirm };
    },
  });
  </script>
  
  <style scoped>
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
  }

  .modal {
    background: white;
    padding: 1.5rem;
    border-radius: 8px;
    min-width: 300px;
    max-width: 500px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }

  header h2 {
    margin: 0;
    font-size: 1.5rem;
    color: #333;
  }
  button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 0.5rem 0.5rem;
  font-size: 1rem;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  }

  button:hover {
    background-color: #0056b3;
  }

  button:disabled {
    background-color: #cccccc;
    cursor: not-allowed;
  }

  footer {
    display: flex;
    justify-content: flex-end;
    gap: 0.5rem;
    margin-top: 1rem;
  }
  </style>
  