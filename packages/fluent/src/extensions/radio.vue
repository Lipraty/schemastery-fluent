<template>
  <schema-base>
    <template #title><slot name="title"></slot></template>
    <template #desc><slot name="desc"></slot></template>
    <template #menu><slot name="menu"></slot></template>
    <template #prefix><slot name="prefix"></slot></template>
    <template #suffix><slot name="suffix"></slot></template>
    <fluent-radio-group
      class="bottom"
      orientation="vertical"
      :value="String(config ?? '')"
      :disabled="disabled || undefined"
      @change="config = ($event.target as HTMLInputElement).value"
    >
      <fluent-radio
        v-for="item in getChoices(schema)"
        :key="item.value"
        :value="item.value"
        :disabled="item.meta.disabled || undefined"
      >
        {{ tt(item.meta.description) || item.value }}
        <k-badge :type="btype" v-for="{ text, type: btype } in item.meta.badges || []">
          {{ t('badge.' + text) }}
        </k-badge>
      </fluent-radio>
    </fluent-radio-group>
  </schema-base>
</template>

<script lang="ts" setup>

import { PropType } from 'vue'
import { useI18n } from 'vue-i18n'
import { getChoices, Schema, useModel, useI18nText } from '../utils'
import SchemaBase from '../base.vue'
import zhCN from '../locales/zh-CN.yml'
import enUS from '../locales/en-US.yml'

defineProps({
  schema: {} as PropType<Schema>,
  modelValue: {} as PropType<any>,
  disabled: {} as PropType<boolean>,
  prefix: {} as PropType<string>,
  initial: {} as PropType<{}>,
})

defineEmits(['update:modelValue'])

const tt = useI18nText()

const config = useModel()

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
