<template>
  <div class="breadcrumb_wrap">
    <ul class="breadcrumb_list">
      <li v-for="breadcrumb in breadcrumbs.data" :key="breadcrumb.name">
        <nuxt-link v-if="breadcrumb.path" :to="breadcrumb.path" class="breadcrumb_link">{{ breadcrumb.name }}</nuxt-link>
        <span v-else class="breadcrumb_name">{{ breadcrumb.name }}</span>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  props: {
    breadcrumbs: Object
  },
  computed: {
    jsonld() {
      const items = Array.prototype.map.call(Object((this.breadcrumbs).data), (item, index) => ({
        '@type': 'ListItem',
        position: index + 1,
        item: {
          '@id': (item.path).toString(),
          name: (item.name).toString(),
        },
      }));
      return  {
        '@context': 'http://schema.org',
        '@type': 'BreadcrumbList',
        itemListElement: items,
      };
    },
  },
  head() {
    const hid = `jsonld-${this._uid}`;
    return {
      script: [
        {
          hid,
          type: 'application/ld+json',
          innerHTML: JSON.stringify(this.jsonld, null, 2),
        },
      ],
      __dangerouslyDisableSanitizersByTagID: {
        [hid]: 'innerHTML',
      },
    };
  },
}
</script>
<style scoped>
.breadcrumb_wrap {
  max-width: 1300px;
  margin: 0 auto 15px;
  padding: 0 15px;
}
.breadcrumb_list {
  display: flex;
  align-items: center;
}
.breadcrumb_list li {
  position: relative;
  padding-right: 30px;
  font-size: 14px;
}
.breadcrumb_list li::after {
  display: block;
  position: absolute;
  top: 50%;
  right: 15px;
  width: 8px;
  height: 8px;
  margin: -4px 0 0;
  border-top: 2px solid rgb(71, 71, 71);
  border-right: 2px solid rgb(71, 71, 71);
  transform: rotate(45deg);
  content: "";
}
.breadcrumb_list li a {
  font-size: 14px;
  font-weight: bold;
  color: rgb(41, 99, 39);
}
.breadcrumb_list li a.nuxt-link-exact-active {
  cursor: auto;
  color: rgb(109, 108, 108);
}
.breadcrumb_list li:last-child::after{
  display: none;
}
@media screen and (max-width: 768px){
  .breadcrumb_wrap {
    padding: 0 15px;
  }
  .breadcrumb_list li a,
  .breadcrumb_list li {
    font-size: 12px;
  }
}
</style>