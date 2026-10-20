<script setup lang="ts">
const siteUrl = 'https://pigeon.abou.dev'
const title = 'Pigeon REST - Free Online API Client, Postman Alternative'
const description =
  'Pigeon is a free, open-source and lightweight online REST API client. Send HTTP requests (GET, POST, PUT, PATCH, DELETE), inspect headers, cookies and responses right in your browser, no install needed.'
const ogImage = `${siteUrl}/og-image.png`

const colorMode = useColorMode()

const favicon = computed(() =>
  colorMode.value === 'dark' ? 'favicon-dark.ico' : 'favicon.ico'
)

function toggleColorMode() {
  colorMode.preference = colorMode.value === 'dark' ? 'light' : 'dark'
}

useFavicon(favicon)

useHead({
  htmlAttrs: { lang: 'en' },
  link: [{ rel: 'canonical', href: `${siteUrl}/` }],
  script: [
    {
      type: 'application/ld+json',
      innerHTML: JSON.stringify({
        '@context': 'https://schema.org',
        '@type': 'WebApplication',
        name: 'Pigeon REST',
        url: `${siteUrl}/`,
        description,
        image: ogImage,
        applicationCategory: 'DeveloperApplication',
        operatingSystem: 'Any',
        browserRequirements: 'Requires JavaScript',
        isAccessibleForFree: true,
        license: 'https://opensource.org/licenses/MIT',
        codeRepository: 'https://github.com/pigeon-rest/pigeon',
        offers: { '@type': 'Offer', price: '0', priceCurrency: 'USD' }
      })
    }
  ]
})

useSeoMeta({
  title,
  description,
  robots: 'index, follow',
  themeColor: [
    { content: '#ffffff', media: '(prefers-color-scheme: light)' },
    { content: '#18181b', media: '(prefers-color-scheme: dark)' }
  ],
  ogType: 'website',
  ogSiteName: 'Pigeon REST',
  ogUrl: `${siteUrl}/`,
  ogLocale: 'en_US',
  ogTitle: title,
  ogDescription: description,
  ogImage,
  ogImageAlt: 'Pigeon REST, a lightweight open-source alternative to Postman',
  ogImageWidth: 1200,
  ogImageHeight: 630,
  twitterCard: 'summary_large_image',
  twitterTitle: title,
  twitterDescription: description,
  twitterImage: ogImage,
  twitterImageAlt:
    'Pigeon REST, a lightweight open-source alternative to Postman'
})

defineShortcuts({
  t: () => toggleColorMode()
})

const { data } = await useFetch<any>(
  'https://api.github.com/repos/pigeon-rest/pigeon'
)

const formatted = computed(() => {
  const count = data.value?.stargazers_count

  if (!count) return '...'

  return count >= 1000
    ? `${(count / 1000).toFixed(1)}k`
    : count.toLocaleString()
})
</script>

<template>
  <UApp>
    <ShortcutsModal />

    <UDashboardNavbar :toggle="false">
      <template #title>
        Pigeon <span class="sr-only">REST - Online API Client</span>
      </template>

      <template #leading>
        <UIcon name="i-ph-bird-fill" class="text-highlighted" />
      </template>

      <template #right>
        <UColorModeButton />

        <UTooltip text="GitHub Stars" arrow>
          <UButton
            to="https://github.com/pigeon-rest/pigeon"
            target="_blank"
            icon="i-ph-github-logo"
            aria-label="Pigeon REST on GitHub"
            size="sm"
            color="neutral"
            variant="ghost"
            :label="formatted"
          />
        </UTooltip>
      </template>
    </UDashboardNavbar>

    <UMain class="flex flex-col">
      <NuxtPage />
    </UMain>
  </UApp>
</template>
