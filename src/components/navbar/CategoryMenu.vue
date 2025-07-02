<template>
  <ul :class="ulClass">
    <li v-for="(item, index) in category" :key="item.categoryTypeId" :class="liClass">
      <a href="javascript:void(0)" class="flex" @click.stop="onClick && onClick(index)">
        {{ item.name }}
        <VueIcon type="mdi" :path="isOpen && isOpen(index) ? mdiChevronUp : mdiChevronDown" size="20" />
      </a>
      <ul v-if="isOpen ? isOpen(index) : true" :class="subUlClass">
        <li
          v-for="child in item.categories"
          :key="child.categoryId"
          class="opacity-60 hover:opacity-100 transition-opacity duration-150"
        >
          <router-link :to="{name:'category',params:{id:child.categoryId}}" class="flex p-2">{{child.name}}</router-link>
        </li>
      </ul>
    </li>
  </ul>
</template>

<script>
import { mdiChevronDown, mdiChevronUp } from '@mdi/js'
export default {
  name: 'CategoryMenu',
  props: {
    category: Array,
    ulClass: String,
    liClass: String,
    subUlClass: String,
    isOpen: Function,
    onClick: Function
  },
  data() {
    return { mdiChevronDown, mdiChevronUp }
  }
}
</script>