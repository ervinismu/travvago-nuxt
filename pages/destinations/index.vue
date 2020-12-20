<template>
  <!-- This example requires Tailwind CSS v2.0+ -->
  <div>
    <!-- This example requires Tailwind CSS v2.0+ -->
    <div class="py-5 px-5">
      <ul
        class="grid grid-cols-1 gap-6 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4"
      >
        <li
          v-for="destination in destinations"
          :key="destination.id"
          class="col-span-1 flex flex-col text-center bg-white rounded-lg shadow divide-y divide-gray-200"
        >
          <div class="flex-1 flex flex-col p-8">
            <img
              class="w-32 h-32 flex-shrink-0 mx-auto bg-black rounded-full"
              :src="destination.image"
              alt=""
            />
            <h3 class="mt-6 text-gray-900 text-sm font-medium">
              {{ destination.name }}
            </h3>
            <dl class="mt-1 flex-grow flex flex-col justify-between">
              <dt class="sr-only">{{ destination.name }}</dt>
              <dd class="text-gray-500 text-sm">{{ destination.address }}</dd>
              <dt class="sr-only">Role</dt>
              <dd class="mt-3">
                <nuxt-link
                  :to="`/destinations/${destination.id}`"
                  class="inline-flex items-center px-3 py-1.5 border border-transparent text-xs font-medium rounded-full shadow-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
                >
                  Visit
                </nuxt-link>
              </dd>
            </dl>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      destinations: null,
    }
  },

  created() {
    fetch('https://travvago-backend.herokuapp.com/api/v1/destination/all')
      .then((response) => response.json())
      .then((data) => {
        this.destinations = data.results
      })
  },
}
</script>
