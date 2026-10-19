<script setup>
import { computed } from 'vue'

const props = defineProps({
  name: {
    type: String,
    required: true
  },
  size: {
    type: [Number, String],
    default: 18
  }
})

const sizePx = computed(() => `${props.size}px`)
</script>

<template>
  <span class="inline-flex items-center justify-center shrink-0" :style="{ width: sizePx, height: sizePx }">
    <!-- PHP -->
    <svg v-if="name === 'PHP'" viewBox="0 0 128 128" :width="size" :height="size">
      <ellipse cx="64" cy="64" rx="60" ry="34" fill="#777BB4" />
      <path d="M42 53h12c5 0 9 2 9 6s-4 7-9 7h-6l-3 10h-6l6-23zm10 8c2 0 4-1 4-2s-1-2-4-2h-5l-1 4h6z" fill="#FFF"/>
      <path d="M64 53h6l-4 13h10l4-13h6l-7 23h-6l4-12H67l-4 12h-6l7-23z" fill="#FFF"/>
      <path d="M89 53h12c5 0 9 2 9 6s-4 7-9 7h-6l-3 10h-6l6-23zm10 8c2 0 4-1 4-2s-1-2-4-2h-5l-1 4h6z" fill="#FFF"/>
    </svg>

    <!-- Laravel -->
    <svg v-else-if="name === 'Laravel'" viewBox="0 0 24 24" :width="size" :height="size" fill="#FF2D20">
      <path d="M7.5 2.5L2 5.5v5l5.5-3 5.5 3v-5L7.5 2.5zM13 8.5l-5.5 3v5l5.5 3 5.5-3v-5L13 8.5zm4-6l-5.5 3 3 1.6 5.5-3-3-1.6zm1 9.5l3.5-1.9v4l-3.5 1.9v-4z" />
    </svg>

    <!-- Node.js -->
    <svg v-else-if="name === 'Node.js'" viewBox="0 0 24 24" :width="size" :height="size" fill="#5FA04E">
      <path d="M12 2l9 5.2v10.4L12 23l-9-5.4V7.2L12 2zm0 2.3L4.8 8.5v7.2L12 20l7.2-4.3V8.5L12 4.3zm-1.2 5.2h2.4v5.8h-2.4V9.5z" />
    </svg>

    <!-- Go -->
    <svg v-else-if="name === 'Go'" viewBox="0 0 24 24" :width="size" :height="size" fill="#00ADD8">
      <path d="M2.5 10.5c.3-3.2 2.8-5.5 6-5.5 2.4 0 4.3 1.3 5.2 3.2l-2.6 1.1c-.5-1.1-1.5-1.8-2.6-1.8-1.9 0-3.3 1.4-3.5 3.3h9v2.5H2.5v-2.8zm14.5 1.5c0-3.5 2.7-6 6-6s6 2.5 6 6-2.7 6-6 6-6-2.5-6-6zm9 0c0-2-1.3-3.5-3-3.5s-3 1.5-3 3.5 1.3 3.5 3 3.5 3-1.5 3-3.5z" transform="scale(0.8) translate(0, 3)"/>
    </svg>

    <!-- HTML5 -->
    <svg v-else-if="name === 'HTML5'" viewBox="0 0 24 24" :width="size" :height="size">
      <path d="M3 2l1.8 17.5L12 22l7.2-2.5L21 2H3zm14.6 5.8l-.2 2.4H8.7l.2 2.5h8l-.6 6-4.3 1.2-4.3-1.2-.3-3.3h2.4l.2 1.6 2 .5 2-.5.2-2.4H6.4l-.7-6.8h11.9z" fill="#E34F26"/>
    </svg>

    <!-- CSS3 -->
    <svg v-else-if="name === 'CSS3'" viewBox="0 0 24 24" :width="size" :height="size">
      <path d="M3 2l1.8 17.5L12 22l7.2-2.5L21 2H3zm14.6 5.8l-.2 2.4H8.7l.2 2.5h8l-.6 6-4.3 1.2-4.3-1.2-.3-3.3h2.4l.2 1.6 2 .5 2-.5.2-2.4H6.4l-.7-6.8h11.9z" fill="#1572B6"/>
    </svg>

    <!-- JavaScript -->
    <svg v-else-if="name === 'JavaScript'" viewBox="0 0 24 24" :width="size" :height="size">
      <rect width="24" height="24" rx="4" fill="#F7DF1E"/>
      <path d="M8.5 17.5c-1.5 0-2.3-.8-2.6-1.8l1.6-.9c.2.6.5.9 1 .9.6 0 1-.3 1-1.1V8.5h2v6.2c0 1.8-1 2.8-3 2.8zm8.2 0c-2 0-3.2-1.2-3.2-3.1 0-1.8 1.1-2.7 2.4-3.2l.6-.2c.5-.2.8-.4.8-.8 0-.5-.4-.8-.9-.8-.6 0-1 .4-1.2.9l-1.6-.9c.5-1.2 1.6-1.8 2.9-1.8 1.9 0 3 1.1 3 2.8 0 1.5-.9 2.4-2.2 2.9l-.6.2c-.6.2-.9.5-.9.9 0 .5.4.9 1 .9.7 0 1.2-.4 1.4-1l1.6.8c-.5 1.5-1.6 2.4-3.2 2.4z" fill="#000"/>
    </svg>

    <!-- Bootstrap -->
    <svg v-else-if="name === 'Bootstrap'" viewBox="0 0 24 24" :width="size" :height="size">
      <rect width="24" height="24" rx="5" fill="#7952B3"/>
      <path d="M8 6h4.5c1.8 0 3 .9 3 2.2 0 1-.6 1.7-1.5 2 1.2.3 2 1.2 2 2.4 0 1.6-1.4 2.6-3.4 2.6H8V6zm2.4 3.7h2c.7 0 1.2-.4 1.2-1s-.5-1-1.2-1h-2v2zm0 3.8h2.3c.8 0 1.4-.4 1.4-1.1 0-.7-.6-1.1-1.4-1.1h-2.3v2.2z" fill="#FFF"/>
    </svg>

    <!-- Vue.js -->
    <svg v-else-if="name === 'Vue.js'" viewBox="0 0 24 24" :width="size" :height="size">
      <path d="M2 3h4.2L12 13.5 17.8 3H22L12 21 2 3z" fill="#42B883"/>
      <path d="M6.2 3h3.6L12 7.1 14.2 3h3.6L12 13.5 6.2 3z" fill="#35495E"/>
    </svg>

    <!-- MySQL -->
    <svg v-else-if="name === 'MySQL'" viewBox="0 0 24 24" :width="size" :height="size" fill="#00758F">
      <path d="M12 2C6.5 2 2 6.5 2 12c0 2.2.7 4.2 1.9 5.8L3 21l3.5-1c1.6 1.2 3.6 2 5.5 2 5.5 0 10-4.5 10-10S17.5 2 12 2zm1 14h-2v-2h2v2zm0-4h-2V7h2v5z"/>
    </svg>

    <!-- MariaDB -->
    <svg v-else-if="name === 'MariaDB'" viewBox="0 0 24 24" :width="size" :height="size" fill="#003545">
      <path d="M12 3C7 3 3 7 3 12s4 9 9 9 9-4 9-9-4-9-9-9zm4 12.5c-.8.6-2 1-3.5 1-2.5 0-4.5-1.5-4.5-4s2-4.5 4.5-4.5c1.5 0 2.7.4 3.5 1l-1 1.5c-.6-.4-1.4-.7-2.3-.7-1.4 0-2.5.9-2.5 2.3 0 1.5 1.1 2.4 2.5 2.4.9 0 1.7-.3 2.3-.7l1 1.7z"/>
    </svg>

    <!-- PostgreSQL -->
    <svg v-else-if="name === 'PostgreSQL'" viewBox="0 0 24 24" :width="size" :height="size" fill="#336791">
      <path d="M12 2C6.5 2 2 6.5 2 12s4.5 10 10 10 10-4.5 10-10S17.5 2 12 2zm3.5 14.5c-1 .6-2.2 1-3.5 1-3 0-5-2-5-5s2-5 5-5c1.3 0 2.5.4 3.5 1l-1.2 1.8c-.7-.5-1.5-.8-2.3-.8-1.8 0-3 1.3-3 3s1.2 3 3 3c.8 0 1.6-.3 2.3-.8l1.2 1.8z"/>
    </svg>

    <!-- MongoDB -->
    <svg v-else-if="name === 'MongoDB'" viewBox="0 0 24 24" :width="size" :height="size">
      <path d="M12 2s-6 6.5-6 12.5C6 18 8.7 22 12 22s6-4 6-7.5C18 8.5 12 2 12 2zm-.8 17.5v-13c-.1 0-3.2 3.6-3.2 8 0 3 1.8 5 3.2 5zm1.6 0c1.4 0 3.2-2 3.2-5 0-4.4-3.1-8-3.2-8v13z" fill="#47A248"/>
    </svg>

    <!-- Git -->
    <svg v-else-if="name === 'Git'" viewBox="0 0 24 24" :width="size" :height="size" fill="#F05032">
      <path d="M21.6 10.9L13.1 2.4a1.8 1.8 0 00-2.5 0L8.2 4.8l3.2 3.2a2.1 2.1 0 012.7 2.7l3.1 3.1a2.1 2.1 0 11-1.3 1.3l-2.9-2.9v4.2a2.1 2.1 0 11-1.8 0v-4.4a2.1 2.1 0 01-1.1-2.7L7 6.1 2.4 10.8a1.8 1.8 0 000 2.5l8.5 8.5a1.8 1.8 0 002.5 0l8.2-8.2a1.8 1.8 0 000-2.7z"/>
    </svg>

    <!-- GitHub -->
    <svg v-else-if="name === 'GitHub'" viewBox="0 0 24 24" :width="size" :height="size" fill="#24292F">
      <path d="M12 2C6.5 2 2 6.5 2 12c0 4.4 2.9 8.2 6.8 9.5.5.1.7-.2.7-.5v-1.8c-2.8.6-3.4-1.4-3.4-1.4-.5-1.1-1.1-1.4-1.1-1.4-.9-.6.1-.6.1-.6 1 .1 1.6 1.1 1.6 1.1.9 1.5 2.4 1.1 3 .8.1-.7.4-1.1.7-1.4-2.2-.3-4.6-1.1-4.6-5 0-1.1.4-2 1-2.7-.1-.3-.5-1.3.1-2.7 0 0 .9-.3 2.8 1.1a9.8 9.8 0 015.2 0c1.9-1.3 2.8-1.1 2.8-1.1.6 1.4.2 2.4.1 2.7.6.7 1 1.6 1 2.7 0 3.9-2.4 4.7-4.6 5 .4.3.7 1 .7 2v3c0 .3.2.6.7.5C19.1 20.2 22 16.4 22 12c0-5.5-4.5-10-10-10z"/>
    </svg>

    <!-- Docker -->
    <svg v-else-if="name === 'Docker'" viewBox="0 0 24 24" :width="size" :height="size" fill="#2496ED">
      <path d="M13.8 9.5H12V7.7h1.8v1.8zm0-2.3H12V5.4h1.8v1.8zm-2.3 2.3H9.7V7.7h1.8v1.8zm0-2.3H9.7V5.4h1.8v1.8zm-2.3 2.3H7.4V7.7h1.8v1.8zm-2.3 0H5.1V7.7h1.8v1.8zm9.2 0h1.8V7.7h-1.8v1.8zM22 12.5c-.3-.2-1.3-.8-2.6-.4-.4-1.6-1.5-2.6-1.6-2.7l-.4.3c.3.5.7 1.4.6 2.3-.4-.2-1-.3-1.6-.2-1.6.2-2.3 1.2-2.7 1.9H2.8c-.5 0-.8.4-.8.8 0 1.8.8 4.2 3.1 5.7 1.9 1.3 4.4 1.4 6.9 1.4 5.3 0 9.4-2.7 9.8-7.8.2-.3.2-.8.2-1.3z"/>
    </svg>

    <!-- Postman -->
    <svg v-else-if="name === 'Postman'" viewBox="0 0 24 24" :width="size" :height="size" fill="#FF6C37">
      <circle cx="12" cy="12" r="10" />
      <path d="M7 12l4-3v6l-4-3zm5.5-3.5L16 12l-3.5 3.5V8.5z" fill="#FFF"/>
    </svg>

    <!-- Fallback -->
    <span v-else class="w-3 h-3 rounded-full bg-teal-500"></span>
  </span>
</template>
