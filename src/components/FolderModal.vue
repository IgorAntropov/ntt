<template>
  <div class="modal" @click.self="closeModal">
    <div class="modal-content">
      <h2>{{ title }}</h2>
      <ul class="folder-tree">
        <folder-node
            v-for="folder in folders"
            :key="folder.id"
            :folder="folder"
            @select="selectFolder"
        ></folder-node>
      </ul>
      <div class="modal-footer">
        <button @click="closeModal">Закрыть</button>
        <button @click="confirmSelection">Ок</button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch } from 'vue';
import type { PropType } from 'vue';
import FolderNode from './FolderNode.vue';

interface Folder {
  id: number;
  name: string;
  children: Folder[];
}

export default defineComponent({
  name: 'FolderModal',
  components: {
    FolderNode,
  },
  props: {
    title: {
      type: String,
      required: true,
    },
    folders: {
      type: Array as PropType<Folder[]>,
      required: true,
    }
  },
  setup(props, { emit }) {
    const selectedFolderId = ref<number | null>(null);

    const closeModal = () => {
      emit('close');
    };

    const confirmSelection = () => {
      if (selectedFolderId.value !== null) {
        emit('select', selectedFolderId.value);
      }
      closeModal();
    };

    const selectFolder = (folderId: number) => {
      selectedFolderId.value = folderId;
    };

    watch(() => props.folders, () => {
      selectedFolderId.value = null;
    });

    return {
      selectedFolderId,
      closeModal,
      confirmSelection,
      selectFolder,
    };
  },
});
</script>

<style scoped>
.modal-footer {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
  margin-top: 20px;
}
</style>
