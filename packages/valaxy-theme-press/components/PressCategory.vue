<script lang="ts" setup>
import { ref } from 'vue'
import type { Category, Post } from 'valaxy'
import { isCategoryList } from 'valaxy'
import { useI18n } from 'vue-i18n'

const props = withDefaults(defineProps<{
  // to eliminate the warning
  category: Category
  level?: number
  displayCategory?: (category: string) => void

  /**
   * collapse children
   */
  collapsable?: boolean
}>(), {
  collapsable: true,
})

const collapsable = ref(props.collapsable)
const { t, locale } = useI18n()

const getTitle = (post: Post | any) => {
  const lang = locale.value
  return post[`title_${lang}`] ? post[`title_${lang}`] : post.title
}
</script>

<template>
  <li
    v-if="category.total"
    p="t-2"
    w="full" border="t t-$pr-c-divider-light"
    class="category-list-item inline-flex items-center justify-between"
  >
    <span class="category-name" font="bold" m="l-1" @click="displayCategory ? displayCategory(category.name) : null">
      {{ category.name === 'Uncategorized' ? t('category.uncategorized') : category.name }}
      <!-- <sup font="normal">[{{ category.total }}]</sup> -->
    </span>
    <span class="folder-action inline-flex cursor-pointer" opacity="50" @click="collapsable = !collapsable">
      <div v-if="collapsable" i-ri-folder-add-line />
      <div v-else i-ri-folder-reduce-line />
    </span>
  </li>

  <template v-if="!collapsable">
    <ul>
      <li v-for="categoryItem, i in category.children" :key="i" class="post-list-item">
        <template v-if="!isCategoryList(categoryItem)">
          <router-link v-if="categoryItem.title" :to="categoryItem.path || ''" class="inline-flex items-center" active-class="active">
            <span m="l-1" text="sm">{{ getTitle(categoryItem) }}</span>
          </router-link>
        </template>

        <PressCategory v-else :category="categoryItem" :display-category="displayCategory" :collapsable="collapsable" />
      </li>
    </ul>
  </template>
</template>
