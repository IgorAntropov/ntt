<template>
  <div class="container">
    <button @click="showModal = true">Открыть</button>
    <span v-if="selectedFolderId" class="selected-folder-id">Выбранная папка: {{ selectedFolderId }}</span>
    <FolderModal
        v-if="showModal"
        :title="modalTitle"
        :folders="folders"
        @close="showModal = false"
        @select="handleSelect"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import FolderModal from './components/FolderModal.vue';

interface Folder {
  id: number;
  name: string;
  children: Folder[];
}

export default defineComponent({
  name: 'App',
  components: {
    FolderModal,
  },
  setup() {
    const showModal = ref(false);
    const selectedFolderId = ref<number | null>(null);
    const modalTitle = ref('Выбор папки');

    const folders: Folder[] = [
      {
        id: 1,
        name: 'Folder 1',
        children: [
          { id: 2, name: 'Subfolder 1.1', children: [
              { id: 3, name: 'Subfolder 1.1.1', children: [
                  { id: 4, name: 'Subfolder 1.1.1.1', children: [
                      { id: 5, name: 'Subfolder 1.1.1.1.1', children: [
                          { id: 6, name: 'Subfolder 1.1.1.1.1.1', children: [
                              { id: 7, name: 'Subfolder 1.1.1.1.1.1.1', children: [
                                  { id: 8, name: 'Subfolder 1.1.1.1.1.1.1.1', children: [
                                      { id: 9, name: 'Subfolder 1.1.1.1.1.1.1.1.1', children: [
                                          { id: 10, name: 'Subfolder 1.1.1.1.1.1.1.1.1.1', children: [] }
                                        ] }
                                    ] }
                                ] }
                            ] }
                        ] }
                    ] }
                ] }
            ] }
        ]
      },
      {
        id: 11,
        name: 'Folder 2',
        children: [
          { id: 12, name: 'Subfolder 2.1', children: [
              { id: 13, name: 'Subfolder 2.1.1', children: [
                  { id: 14, name: 'Subfolder 2.1.1.1', children: [
                      { id: 15, name: 'Subfolder 2.1.1.1.1', children: [
                          { id: 16, name: 'Subfolder 2.1.1.1.1.1', children: [] }
                        ] }
                    ] }
                ] }
            ] }
        ]
      }
    ];

    const handleSelect = (folderId: number) => {
      selectedFolderId.value = folderId;
      showModal.value = false;
    };

    return {
      showModal,
      selectedFolderId,
      modalTitle,
      folders,
      handleSelect,
    };
  },
});
</script>

<style>
.selected-folder-id {
  margin-left: 10px;
  color: blue;
}
</style>
