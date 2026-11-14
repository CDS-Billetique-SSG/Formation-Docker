<script setup lang="ts">
/**
 * Comparaison "Sans Docker" (liste d'étapes pénibles) vs "Avec Docker".
 */
withDefaults(defineProps<{
  before: string[]
  after: string[]
  step?: number
}>(), { step: 99 })
</script>

<template>
  <div class="grid grid-cols-2 gap-6">
    <div class="side ko">
      <div class="head">😩 Sans Docker</div>
      <ol>
        <li v-for="(b, i) in before" :key="i">
          <span class="num">{{ i + 1 }}</span>
          <span v-html="b" />
        </li>
      </ol>
    </div>
    <Transition name="fade-up">
      <div v-if="step >= 1" class="side ok">
        <div class="head">😎 Avec Docker</div>
        <ol>
          <li v-for="(a, i) in after" :key="i">
            <span class="num">{{ i + 1 }}</span>
            <span v-html="a" />
          </li>
        </ol>
        <slot />
      </div>
    </Transition>
  </div>
</template>

<style scoped>
.side {
  border-radius: 14px;
  padding: 16px 18px;
  border: 1px solid;
}
.ko { background: rgba(239, 68, 68, 0.07); border-color: rgba(239, 68, 68, 0.4); }
.ok { background: rgba(16, 185, 129, 0.07); border-color: rgba(16, 185, 129, 0.4); }
.head { font-weight: 700; font-size: 1.05rem; margin-bottom: 10px; }
ol { list-style: none; padding: 0; margin: 0; display: flex; flex-direction: column; gap: 6px; }
li { display: flex; gap: 10px; align-items: flex-start; font-size: 0.85rem; line-height: 1.35; margin: 0 !important; }
.num {
  flex: none;
  width: 22px; height: 22px;
  border-radius: 50%;
  display: grid; place-items: center;
  font-size: 0.7rem; font-weight: 700;
}
.ko .num { background: rgba(239, 68, 68, 0.25); color: #fca5a5; }
.ok .num { background: rgba(16, 185, 129, 0.25); color: #6ee7b7; }
:deep(code) { font-size: 0.78rem; }
</style>
