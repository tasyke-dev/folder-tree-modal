<template>
  <ul>
    <FolderTreeNode
      v-for="folder in folders"
      :key="folder.id"
      :node="folder"
      :selectedId="selectedFolderId ?? undefined"
      @select="selectFolder"
    />
  </ul>
</template>

<script lang="ts">
import { defineComponent, PropType, ref } from 'vue';
import FolderTreeNode from './FolderTreeNode.vue';
import { Folder } from '@/types/folder'; 

export default defineComponent({
  name: 'FolderTree',
  components: { FolderTreeNode },
  props: {
    folders: {
      type: Array as PropType<Folder[]>,
      required: true,
    },
  },
  emits: ['select'],
  setup(props, { emit }) {
    const selectedFolderId = ref<number | null>(null);

    const selectFolder = (id: number) => {
      selectedFolderId.value = id;
      emit('select', id);
    };

    return {
      selectedFolderId,
      selectFolder,
    };
  },
});
</script>

<style scoped>
ul {
  list-style: none;
  padding: 0;
  margin: 0;
}
</style>
