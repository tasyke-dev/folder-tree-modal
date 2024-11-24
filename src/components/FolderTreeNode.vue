<template>
  <li>
    <div
      :class="{ selected: isSelected }"
      @click="selectFolder"
    >
      <span @click.stop="toggleNode" class="toggle-icon">
        {{ isOpen ? '-' : '+' }}
      </span>
      {{ node.name }}
    </div>
    <ul v-if="isOpen && node.children && node.children.length">
      <FolderTreeNode
        v-for="child in node.children"
        :key="child.id"
        :node="child"
        :selectedId="selectedId"
        @select="handleSelect"
      />
    </ul>
  </li>
</template>

<script lang="ts">
import { defineComponent, ref, PropType, computed } from 'vue';
import { Folder } from '@/types/folder';

export default defineComponent({
  name: 'FolderTreeNode',
  props: {
    node: {
      type: Object as PropType<Folder>,
      required: true,
    },
    selectedId: {
      type: Number,
      required: false,
    },
  },
  emits: ['select'],
  setup(props, { emit }) {
    const isOpen = ref(false);

    const toggleNode = () => {
      isOpen.value = !isOpen.value;
    };

    const selectFolder = () => {
      emit('select', props.node.id);
    };

    const handleSelect = (id: number) => {
      emit('select', id);
    };

    const isSelected = computed(() => props.selectedId === props.node.id);

    return {
      isOpen,
      toggleNode,
      selectFolder,
      handleSelect,
      isSelected,
    };
  },
});
</script>

<style scoped>
li {
  list-style-type: none;
  margin: 0.5rem 0;
}

li div {
  padding: 0.5rem;
  border-radius: 4px;
  transition: background-color 0.3s ease;
  cursor: pointer;
  display: flex;
  align-items: center;
}

li div:hover {
  background-color: #f0f0f0;
}

li div.selected {
  background-color: #0056b3;
  color: white;
}

.toggle-icon {
  margin-right: 8px;
  cursor: pointer;
  font-weight: bold;
  color: #007bff;
}
</style>
