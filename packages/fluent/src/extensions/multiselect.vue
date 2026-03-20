<template>
  <schema-base>
    <template #title><slot name="title"></slot></template>
    <template #desc><slot name="desc"></slot></template>
    <template #menu>
      <slot name="menu"></slot>
      <div class="k-menu-separator"></div>
      <div class="k-menu-item" :class="{ disabled }" @click="selectAll">
        <span class="k-menu-icon"><icon-square-check></icon-square-check></span>
        {{ t('select.all') }}
      </div>
      <div class="k-menu-item" :class="{ disabled }" @click="selectNone">
        <span class="k-menu-icon"><icon-square-empty></icon-square-empty></span>
        {{ t('select.none') }}
      </div>
    </template>
    <template #prefix><slot name="prefix"></slot></template>
    <template #suffix><slot name="suffix"></slot></template>
    <template #control>
      <div class="k-multiselect-checkboxes">
        <fluent-checkbox
          v-for="item in items"
          :key="item.value"
          :checked="values.includes(item.value) || undefined"
          :disabled="item.meta.disabled || disabled || undefined"
          @change="toggle(item.value)"
        >
          {{ tt(item.meta.description) || item.value }}
          <k-badge :type="btype" v-for="{ text, type: btype } in item.meta.badges || []">
            {{ t('badge.' + text) }}
          </k-badge>
        </fluent-checkbox>
      </div>
    </template>
  </schema-base>
</template>

<script lang="ts" setup>

import { PropType } from 'vue'
import { useI18n } from 'vue-i18n'
import { Schema, useI18nText, useMultiSelect } from '../utils'
import SchemaBase from '../base.vue'
import zhCN from '../locales/zh-CN.yml'
import enUS from '../locales/en-US.yml'
import { IconSquareCheck, IconSquareEmpty } from '../icons'

defineProps({
  schema: {} as PropType<Schema>,
  modelValue: {} as PropType<number>,
  disabled: {} as PropType<boolean>,
  prefix: {} as PropType<string>,
  initial: {} as PropType<{}>,
})

defineEmits(['update:modelValue'])

const tt = useI18nText()

const { values, items, toggle, selectAll, selectNone } = useMultiSelect()

const { t, setLocaleMessage } = useI18n({
  messages: {
    'zh-CN': zhCN,
    'en-US': enUS,
  },
})

if (import.meta.hot) {
  import.meta.hot.accept('../locales/zh-CN.yml', (module) => {
    setLocaleMessage('zh-CN', module.default)
  })
  import.meta.hot.accept('../locales/en-US.yml', (module) => {
    setLocaleMessage('en-US', module.default)
  })
}

</script>

<style lang="scss">

.k-multiselect-checkboxes {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}

</style>
