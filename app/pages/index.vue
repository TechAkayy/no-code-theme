<script setup lang="ts">
  import { heroImageUrl } from '@/utils/hero'
  import { useTimeAgo } from '@vueuse/core'

  definePageMeta({
    // layout: 'default',
    // name: 'index',
    // alias: 'index',
    title: 'Home',
    description: 'Sip, Savor, and Spark Ideas at Cafe Bella Vista!',
    navOrder: 1,
    type: 'primary',
    icon: 'i-mdi-home',
    // ogImage: 'images/ogImage.png', // url or local images inside public folder, for eg, ~/public/images/ogImage.png
  })

  const { optimizeImage } = useOptimizeImage()
  const heroImageOptimized = {
    alt: `hero`,
    cover: true,
    ...optimizeImage(
      heroImageUrl,
      /* options */
      {
        /* If using local images instead of unsplash url, enable netlify provider */
        // provider:
        //     process.env.NODE_ENV === 'production'
        //       ? 'netlify'
        //       : null /* defaults to ipx or ipxStatic */,
        placeholder: false, // placeholder image before the actual image is fully loaded.
      },
      true /* return bgStyles */,
    ),
  }

  const heroImage = heroImageOptimized.src
  const bgStyles = heroImageOptimized.bgStyles

  const route = useRoute()
  const config = useRuntimeConfig()

  const {
    data: posts,
    refresh,
    pending,
  } = await useFetch(config.public.wordpressUrl, {
    method: 'get',
    query: {
      query: `
        query NewQuery {
          posts(first: 10) {
            nodes {
              title
              date
              excerpt
              uri
              author {
                node {
                  name
                  avatar {
                    url
                  }
                }
              }
              tags {
                nodes {
                  name
                }
              }
            }
          }
        }`,
    },
    transform(data) {
      const posts = data.data.posts.nodes.map((post) => ({
        ...post,
        timeAgo: useTimeAgo(new Date(post.date)),
      }))
      return posts
    },
  })
</script>
<template>
  <div>
    <section
      class="bg-white dark:bg-gray-900"
      data-pg-name="Visual image with heading"
      data-pg-collapsed
    >
      <div
        class="grid grid-cols-[1fr] max-w-screen-xl mx-auto px-4 py-8 lg:gap-8 lg:grid-cols-12 lg:py-16 xl:gap-0"
      >
        <div
          class="lg:col-end-8 lg:col-start-1 lg:row-end-auto lg:row-start-auto mr-auto place-self-center"
          data-pg-collapsed
        >
          <h1
            class="font-extrabold leading-none max-w-2xl mb-6 text-4xl tracking-tight dark:text-white md:text-5xl xl:text-6xl"
          >
            Payments tool for software companies
          </h1>
          <p
            class="max-w-2xl mb-6 font-light text-gray-500 lg:mb-8 md:text-lg lg:text-xl dark:text-gray-400"
          >
            From checkout to global sales tax compliance, companies around the
            world use Flowbite to simplify their payment stack.
          </p>
          <a
            href="#"
            class="inline-flex items-center justify-center px-5 py-3 mr-3 text-base font-medium text-center text-white rounded-lg bg-primary-700 hover:bg-primary-800 focus:ring-4 focus:ring-primary-300 dark:focus:ring-primary-900"
          >
            Get started
            <svg
              class="w-5 h-5 ml-2 -mr-1"
              fill="currentColor"
              viewBox="0 0 20 20"
              xmlns="http://www.w3.org/2000/svg"
            >
              <path
                fill-rule="evenodd"
                d="M10.293 3.293a1 1 0 011.414 0l6 6a1 1 0 010 1.414l-6 6a1 1 0 01-1.414-1.414L14.586 11H3a1 1 0 110-2h11.586l-4.293-4.293a1 1 0 010-1.414z"
                clip-rule="evenodd"
              ></path>
            </svg>
          </a>
          <a
            href="#"
            class="inline-flex items-center justify-center px-5 py-3 text-base font-medium text-center text-gray-900 border border-gray-300 rounded-lg hover:bg-gray-100 focus:ring-4 focus:ring-gray-100 dark:text-white dark:border-gray-700 dark:hover:bg-gray-700 dark:focus:ring-gray-800"
          >
            Speak to Sales
          </a>
        </div>
        <div
          class="hidden lg:col-end-13 lg:col-start-8 lg:flex lg:mt-0 lg:row-end-auto lg:row-start-auto"
        >
          <NuxtImg
            src="https://flowbite.s3.amazonaws.com/blocks/marketing-ui/hero/phone-mockup.png"
            alt="mockup"
          />
        </div>
      </div>
    </section>
    <section>
      <div class="py-8 px-4 mx-auto max-w-screen-xl lg:py-16 lg:px-6">
        <div class="mx-auto max-w-screen-sm text-center lg:mb-16 mb-8">
          <h2
            class="mb-4 text-3xl lg:text-4xl tracking-tight font-extrabold text-gray-900 dark:text-white"
          >
            Our Blog
          </h2>
          <p class="font-light text-gray-500 sm:text-xl dark:text-gray-400">
            We use an agile approach to test assumptions and connect with the
            needs of your audience early and often.
          </p>
        </div>
        <div class="grid gap-8 lg:grid-cols-2">
          <ArticleCard
            v-for="(post, index) in posts"
            :key="index"
            :post="post"
          />
        </div>
      </div>
    </section>
  </div>
</template>
<style scoped></style>
