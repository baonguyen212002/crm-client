<!-- ~/pages/profile.vue -->
<template>
  <div class="max-w-4xl mx-auto p-6">
    <div v-if="loading" class="flex justify-center items-center min-h-[200px]">
      <div class="animate-spin rounded-full h-12 w-12 border-b-2 border-gray-900"></div>
    </div>

    <div v-else-if="error" class="bg-red-100 border-l-4 border-red-500 text-red-700 p-4" role="alert">
      <p class="font-bold">Error</p>
      <p>{{ error.message }}</p>
    </div>

    <div v-else-if="data" class="bg-white shadow rounded-lg">
      <div class="px-4 py-5 sm:px-6">
        <h3 class="text-lg leading-6 font-medium text-gray-900">User Profile</h3>
        <p class="mt-1 max-w-2xl text-sm text-gray-500">Personal details and information</p>
      </div>

      <div class="border-t border-gray-200">
        <dl>
          <div class="bg-gray-50 px-4 py-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-6">
            <dt class="text-sm font-medium text-gray-500">Full name</dt>
            <dd class="mt-1 text-sm text-gray-900 sm:col-span-2 sm:mt-0">{{ data.me.name }}</dd>
          </div>

          <div class="bg-white px-4 py-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-6">
            <dt class="text-sm font-medium text-gray-500">Email address</dt>
            <dd class="mt-1 text-sm text-gray-900 sm:col-span-2 sm:mt-0">{{ data.me.email }}</dd>
          </div>

          <div class="bg-gray-50 px-4 py-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-6">
            <dt class="text-sm font-medium text-gray-500">Email verified</dt>
            <dd class="mt-1 text-sm text-gray-900 sm:col-span-2 sm:mt-0">
              <span v-if="data.me.email_verified_at" class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-green-100 text-green-800">
                Verified on {{ formatDate(data.me.email_verified_at) }}
              </span>
              <span v-else class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-yellow-100 text-yellow-800">
                Not verified
              </span>
            </dd>
          </div>
        </dl>
      </div>
    </div>
  </div>
</template>

<script setup>
import {useGetUserData} from '@/composables/useQueryGraphql'
import profileFav from '@/assets/images/profile-fav.png';
const route = useRoute();

const { data, loading, error } = useGetUserData()

const formatDate = (dateString) => {
  if (!dateString) return ''
  return new Date(dateString).toLocaleDateString('en-US', {
    year: 'numeric',
    month: 'long',
    day: 'numeric',
  })
}
const segments = route.fullPath.split('/').filter(Boolean);
const title = ref(segments)
useHead({
  link: [
    { rel: 'icon', type: 'image/png', href: profileFav }, // Cập nhật favicon
  ]
});
useSeoMeta({
  title: title.value,
  ogTitle: title.value,
  description: 'This is profile.',
  ogDescription: 'This is profile.',
  ogImage: {
    width: 1200,
    url: profileFav
  },
  twitterCard: 'summary_large_image',
})
definePageMeta({
  key: route => route.fullPath,
  title: 'Profile',
  description: 'User profile page.',
  keywords: ['profile', 'user', 'details'],
  pageTransition: { name: 'fade', mode: 'out-in' },
  middleware: 'auth',
  keepalive: true,
  layout: 'default',
  scrollToTop: true
});
</script>