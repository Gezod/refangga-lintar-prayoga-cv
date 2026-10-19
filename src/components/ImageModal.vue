<script setup>
import { onMounted, onUnmounted } from 'vue'
import { X, ExternalLink, Maximize2 } from '@lucide/vue'

const props = defineProps({
  isOpen: {
    type: Boolean,
    default: false
  },
  image: {
    type: Object,
    default: () => ({ url: '', title: '', caption: '', projectTitle: '' })
  }
})

const emit = defineEmits(['close'])

function handleKeyDown(e) {
  if (e.key === 'Escape' && props.isOpen) {
    emit('close')
  }
}

onMounted(() => {
  window.addEventListener('keydown', handleKeyDown)
})

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeyDown)
})
</script>

<template>
  <Teleport to="body">
    <Transition name="modal-fade">
      <div
        v-if="isOpen"
        class="fixed inset-0 z-50 flex items-center justify-center p-4 sm:p-6 bg-slate-900/80 backdrop-blur-md"
        @click.self="emit('close')"
      >
        <div class="relative w-full max-w-5xl bg-white rounded-2xl shadow-2xl overflow-hidden border border-slate-200 animate-scale-in">
          <!-- Modal Header -->
          <div class="flex items-center justify-between px-6 py-4 border-b border-slate-100 bg-slate-50">
            <div class="flex items-center gap-3">
              <span class="w-3 h-3 rounded-full bg-red-400"></span>
              <span class="w-3 h-3 rounded-full bg-amber-400"></span>
              <span class="w-3 h-3 rounded-full bg-emerald-400"></span>
              <div class="ml-2 font-mono text-xs text-slate-500 font-medium">
                {{ image.projectTitle }} <span class="text-teal-600">/</span> {{ image.title }}
              </div>
            </div>
            <button
              @click="emit('close')"
              class="p-1.5 rounded-lg text-slate-400 hover:text-slate-700 hover:bg-slate-200 transition-colors"
              title="Close (Esc)"
            >
              <X :size="20" />
            </button>
          </div>

          <!-- Image Container -->
          <div class="relative max-h-[75vh] overflow-auto bg-slate-950 flex items-center justify-center p-2">
            <img
              :src="image.url"
              :alt="image.title"
              class="max-w-full max-h-[70vh] object-contain rounded shadow-lg"
            />
          </div>

          <!-- Modal Footer / Caption -->
          <div class="px-6 py-3.5 bg-white border-t border-slate-100 flex flex-col sm:flex-row sm:items-center sm:justify-between gap-2">
            <div>
              <h4 class="text-sm font-bold text-slate-900">{{ image.title }}</h4>
              <p class="text-xs text-slate-500 mt-0.5">{{ image.caption }}</p>
            </div>
            <a
              :href="image.url"
              target="_blank"
              class="inline-flex items-center gap-1 text-xs font-mono font-medium text-teal-600 hover:text-teal-700 hover:underline"
            >
              Open Original <ExternalLink :size="12" />
            </a>
          </div>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<style scoped>
.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity 0.25s ease;
}
.modal-fade-enter-from,
.modal-fade-leave-to {
  opacity: 0;
}

@keyframes scaleIn {
  from {
    transform: scale(0.96);
    opacity: 0;
  }
  to {
    transform: scale(1);
    opacity: 1;
  }
}
.animate-scale-in {
  animation: scaleIn 0.25s cubic-bezier(0.16, 1, 0.3, 1) forwards;
}
</style>
