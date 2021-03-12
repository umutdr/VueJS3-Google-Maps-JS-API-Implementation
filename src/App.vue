<template>
  <div id="nav">
    <div v-for="(navItem, Index) in navItems" :key="Index" class="nav-item">
      <router-link :to="navItem.to">{{ navItem.name }}</router-link>
    </div>
  </div>
  <router-view />
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      navItems: [
        {
          name: 'Home',
          to: { name: 'Home' },
        },
      ],
    };
  },
};
</script>
