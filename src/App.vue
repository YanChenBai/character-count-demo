<script setup lang="ts">
import { CharacterCount as CharacterCountFix } from '@/tiptap-extension/character-count'
import CharacterCount from '@tiptap/extension-character-count'
import StarterKit from '@tiptap/starter-kit'
import { EditorContent, useEditor } from '@tiptap/vue-3'
import { computed } from 'vue'

type CharacterCountType = typeof CharacterCount

function useTiptapEditor(CharacterCountExtension: CharacterCountType, limit = 10) {
  const editor = useEditor({
    content: '<p>ABCDE</p>',
    extensions: [
      StarterKit,
      CharacterCountExtension.configure({
        limit,
      }),
    ],
  })

  const count = computed(() => editor.value?.storage.characterCount.characters() ?? 0)

  return [count, editor]
}

const [count, editor] = useTiptapEditor(CharacterCount)
const [count2, editor2] = useTiptapEditor(CharacterCountFix)
</script>

<template>
  <div class="wrap">
    <div>
      <div class="head">
        <div>Normal</div>
        <div>{{ count }} / 10</div>
      </div>
      <EditorContent :editor="editor" class="editor" />
    </div>

    <div>
      <div class="head">
        <div>Fix</div>
        <div>{{ count2 }} / 10</div>
      </div>
      <EditorContent :editor="editor2" class="editor" />
    </div>
  </div>
</template>

<style scoped>
.wrap {
  max-width: 500px;
  margin: 0 auto;
  padding: 80px 20px 20px 20px;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  gap:38px
}

.head {
  display: flex;
  justify-content: space-between;
  margin-bottom: 8px;
}

.head div:nth-child(1){
  font-weight: 600;
  font-size: 18px;
}

:deep(.tiptap) {
  background-color: rgba(255,255,255,0.1);
  padding: 10px;
  border-radius: 6px;
  box-sizing: border-box;
}
</style>
