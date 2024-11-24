<template>
  <div id="app">
    <button @click="openModal">Открыть</button>
    <ModalWindow
      v-if="isModalOpen"
      title="Выберите папку"
      @close="closeModal"
      @confirm="handleConfirm"
    >
      <FolderTree :folders="folders" @select="selectFolder" />
    </ModalWindow>
    <p v-if="selectedFolderId">Вы выбрали папку с ID: {{ selectedFolderId }}</p>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import ModalWindow from './components/ModalWindow.vue';
import FolderTree from './components/FolderTree.vue';
import { Folder } from './types/folder';

const mockFolders: Folder[] = [
  {
    id: 1,
    name: 'Папка 1',
    children: [
      { id: 2, name: 'Папка 1.1', children: [] },
      {
        id: 3,
        name: 'Папка 1.2',
        children: [{ id: 4, name: 'Папка 1.2.1', children: [] }],
      },
    ],
  },
  {
    id: 5,
    name: 'Папка 2',
    children: [
      { id: 6, name: 'Папка 2.1', children: [] },
      {
        id: 7,
        name: 'Папка 2.2',
        children: [{ id: 8, name: 'Папка 2.2.1', children: [] }],
      },
    ],
  },
];

export default defineComponent({
  components: { ModalWindow, FolderTree },
  setup() {
    const isModalOpen = ref(false);
    const selectedFolderId = ref<number | null>(null);
    const temporarySelectedId = ref<number | null>(null);

    const openModal = () => {
      isModalOpen.value = true;
      temporarySelectedId.value = null;
    };

    const closeModal = () => {
      isModalOpen.value = false;
    };

    const selectFolder = (folderId: number) => {
      temporarySelectedId.value = folderId;
    };

    const handleConfirm = () => {
      if (temporarySelectedId.value !== null) {
        selectedFolderId.value = temporarySelectedId.value;
      }
      closeModal();
    };

    return {
      isModalOpen,
      openModal,
      closeModal,
      selectFolder,
      handleConfirm,
      selectedFolderId,
      folders: mockFolders,
    };
  },
});
</script>
<style scoped>
button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 0.5rem 1rem;
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
</style>