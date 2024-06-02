<template>
  <li>
    <div @click="toggle">
      <span v-if="folder.children.length">
        <span>{{ open ? '▼' : '▶' }}</span>
      </span>
      <span>📁</span>
      {{ folder.name }}
    </div>
    <ul v-show="open">
      <folder-node
          v-for="child in folder.children"
          :key="child.id"
          :folder="child"
          :selected-folder-id="selectedFolderId"
          @select="$emit('select', $event)"
      ></folder-node>
    </ul>
  </li>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

interface Folder {
  id: number;
  name: string;
  children: Folder[];
}

export default defineComponent({
  name: 'FolderNode',
  props: {
    folder: {
      type: Object as () => Folder,
      required: true,
    },
    selectedFolderId: {
      type: Number,
      default: null,
    },
  },
  setup(props, { emit }) {
    const open = ref(false);

    const toggle = () => {
      open.value = !open.value;

      emit('select', props.folder.id);
    };

    return {
      open,
      toggle,
    };
  },
});
</script>

<style scoped>
</style>
