<template>
  <HeadlessTransitionRoot as="template" :show="isShown">
    <HeadlessDialog as="div" class="relative z-50" @close="close">
      <HeadlessTransitionChild as="template" enter="ease-in-out duration-500" enter-from="opacity-0" enter-to="opacity-100" leave="ease-in-out duration-500" leave-from="opacity-100" leave-to="opacity-0">
        <div class="fixed inset-0 bg-gray-500 dark:bg-black bg-opacity-75 dark:bg-opacity-50 transition-opacity backdrop-blur-sm" />
      </HeadlessTransitionChild>
      <div class="fixed inset-0 z-10 w-screen overflow-y-auto p-4 sm:p-6 md:px-20 md:py-10">
        <HeadlessTransitionChild as="template" enter="ease-out duration-300" enter-from="opacity-0 scale-95" enter-to="opacity-100 scale-100" leave="ease-in duration-200" leave-from="opacity-100 scale-100" leave-to="opacity-0 scale-95">
          <HeadlessDialogPanel class="mx-auto h-full transform divide-y divide-gray-100 overflow-hidden rounded-xl bg-white dark:bg-neutral-800 shadow-2xl ring-1 ring-black ring-opacity-5 transition-all">
            <component :is="opts.component" v-bind="opts.componentProps" />
          </HeadlessDialogPanel>
        </HeadlessTransitionChild>
      </div>
    </HeadlessDialog>
  </HeadlessTransitionRoot>
</template>

<script setup>

// PROPS / EMITS

defineProps({
  opts: {
    type: Object,
    default: () => ({}),
    required: true
  },
  isShown: {
    type: Boolean,
    default: false,
    required: true
  }
})

const emit = defineEmits(['update:isShown', 'closeOk', 'closeCancel'])

// PROVIDE

provide('overlayModalMethods', {
  ok: (val) => {
    emit('update:isShown', false)
    emit('closeOk', val)
  },
  cancel: (val) => {
    emit('update:isShown', false)
    emit('closeCancel', val)
  }
})

// METHODS

function close () {
  emit('update:isShown', false)
  emit('closeCancel')
}
</script>