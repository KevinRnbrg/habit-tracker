<template>
  <v-footer class="bg-black">
      <v-row justify="center" no-gutters>
          <v-btn
              v-for="link in links"
              :key="link.text"
              class="mx-2"
              color="white"
              rounded="xl"
              variant="text"
              :href="link.url"
              target="_blank"
          >
              {{ link.text }}
          </v-btn>
          <v-col class="text-center mt-4" cols="12">
              {{ new Date().getFullYear() }} — <strong>Kevin Rosenberg</strong>
          </v-col>
      </v-row>
  </v-footer>
</template>

<script>
export default {
  data: () => ({
      links: [
          { text: 'About', url: '' }, // open a modal with info about the project
          { text: 'LinkedIn', url: 'https://www.linkedin.com/in/rosenbergkevin/' },
          { text: 'GitHub', url: 'https://github.com/KevinRnbrg' }
      ],
  }),
}
</script>
