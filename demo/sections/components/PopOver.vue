

<template>
  <ComponentPage
    title="Pop Over"
    :demos="[
      { title: 'Manual' },
      { title: 'Dynamic' },
      { title: 'Grouped' },
    ]"
  >
    <template #description>
      This is where we add a short description of <p-code inline>
        p-pop-over
      </p-code>. Describe the components intent, not hyper specific documentation that belongs on vitepress page.
    </template>

    <template #manual>
      <div class="flex items-center gap-2">
        <position-select v-model:position="position" />
        <p-toggle v-model="showManual">
          <template #append>
            Show
          </template>
        </p-toggle>
      </div>
      <div ref="manualContainer" class="relative bg-slate-500 p-2 my-2">
        <div>
          container
        </div>
        <div ref="manualTarget" class="inline-block bg-black text-white m-20 p-2">
          target
        </div>
        <template v-if="showManual">
          <div ref="manualContent" class="bg-primary text-white p-2" :style="manualStyles">
            <span>Manual Composition Content</span>
          </div>
        </template>
      </div>

      {{ manualStyles }}
    </template>

    <template #dynamic>
      <div class="flex items-center gap-2">
        <position-select v-model:position="positions" />
        <p-toggle v-model="showDynamic">
          <template #append>
            Show
          </template>
        </p-toggle>
      </div>
      <div ref="dynamicContainer" class="relative bg-slate-500 p-2 my-2">
        <div>
          container
        </div>
        <div ref="dynamicTarget" class="inline-block bg-black text-white m-20 p-2">
          target
        </div>
        <template v-if="showDynamic">
          <div ref="dynamicContent" class="bg-primary text-white p-20" :style="dynamicStyles">
            <span>Dynamic Composition Content</span>
          </div>
        </template>
      </div>
      {{ dynamicStyles }}
    </template>

    <template #grouped>
      <p-content>
        <template v-for="group in ['Group 1', 'Group 2']" :key="group">
          <div @mouseleave="close">
            <p-heading heading="3">
              {{ group }}
            </p-heading>
            <div class="flex gap-2 items-center flex-wrap">
              <template v-for="i in 5" :key="i">
                <p-pop-over :group="group" :placement="top">
                  <template #target="{ open }">
                    <p-button @mouseover="open">
                      Open {{ i }}
                    </p-button>
                  </template>

                  <p-card>
                    Hello there {{ i }}
                  </p-card>
                </p-pop-over>
              </template>
            </div>
          </div>
        </template>
      </p-content>
    </template>
  </ComponentPage>
</template>

<script lang="ts" setup>
  import { usePositionStyles, useMostVisiblePositionStyles } from '@/compositions/position'
  import { usePopOverGroup } from '@/compositions/usePopOverGroup'
  import { left, right, top } from '@/utilities/position'
  import { ref } from 'vue'
  import ComponentPage from '@/demo/components/ComponentPage.vue'
  import PositionSelect from '@/demo/components/PositionSelect.vue'

  const position = ref(left)
  const showManual = ref(false)
  const {
    target: manualTarget,
    content: manualContent,
    container: manualContainer,
    styles: manualStyles,
  } = usePositionStyles(position)

  const positions = ref([left, right])
  const showDynamic = ref(false)
  const {
    target: dynamicTarget,
    content: dynamicContent,
    container: dynamicContainer,
    styles: dynamicStyles,
  } = useMostVisiblePositionStyles(positions)

  const { close: closeGroupOne } = usePopOverGroup('Group 1')
  const { close: closeGroupTwo } = usePopOverGroup('Group 2')

  function close(): void {
    closeGroupOne()
    closeGroupTwo()
  }
</script>