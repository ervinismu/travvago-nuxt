<template>
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <div class="max-w-5xl mx-auto py-5">
      <div>
        <div>
          <h3 class="text-lg leading-6 font-medium text-gray-900">
            Your Destination
          </h3>
        </div>

        <div class="mt-5 border-t border-gray-200">
          <dl class="sm:divide-y sm:divide-gray-200">
            <div class="py-4 sm:py-5 sm:grid sm:grid-cols-3 sm:gap-4">
              <dt class="text-sm font-medium text-gray-500">Name</dt>
              <dd class="mt-1 text-sm text-gray-900 sm:mt-0 sm:col-span-2">
                {{ destination.name }}
              </dd>
            </div>
            <div class="py-4 sm:py-5 sm:grid sm:grid-cols-3 sm:gap-4">
              <dt class="text-sm font-medium text-gray-500">About</dt>
              <dd class="mt-1 text-sm text-gray-900 sm:mt-0 sm:col-span-2">
                {{ destination.about }}
              </dd>
            </div>
            <div class="py-4 sm:py-5 sm:grid sm:grid-cols-3 sm:gap-4">
              <dt class="text-sm font-medium text-gray-500">Address</dt>
              <dd class="mt-1 text-sm text-gray-900 sm:mt-0 sm:col-span-2">
                {{ destination.address }}
              </dd>
            </div>
            <div class="py-4 sm:py-5 sm:grid sm:grid-cols-3 sm:gap-4">
              <dt class="text-sm font-medium text-gray-500">Weather</dt>
              <dd class="mt-1 text-sm text-gray-900 sm:mt-0 sm:col-span-2">
                <!-- This example requires Tailwind CSS v2.0+ -->
                <div class="sm:flex">
                  <div class="mb-4 flex-shrink-0 sm:mb-0 sm:mr-4">
                    <svg
                      class="h-20 w-full sm:w-32 bg-white text-indigo-300"
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 20 20"
                      fill="currentColor"
                    >
                      <path
                        d="M5.5 16a3.5 3.5 0 01-.369-6.98 4 4 0 117.753-1.977A4.5 4.5 0 1113.5 16h-8z"
                      />
                    </svg>
                  </div>
                  <div>
                    <p class="mt-1">Name : {{ name }}</p>
                    <p class="mt-1">Coordinate : {{ lat }} / {{ lon }}</p>
                    <p class="mt-1">
                      Current Temp : {{ currentTemp }} / {{ overcast }}
                    </p>
                  </div>
                </div>
              </dd>
            </div>
            <div class="py-4 sm:py-5 sm:grid sm:grid-cols-3 sm:gap-4">
              <dt class="text-sm font-medium text-gray-500">Opening Hours</dt>
              <dd class="mt-1 text-sm text-gray-900 sm:mt-0 sm:col-span-2">
                <ul
                  class="border border-gray-200 rounded-md divide-y divide-gray-200"
                >
                  <li
                    v-for="hour in opening_hours"
                    :key="hour.id"
                    class="pl-3 pr-4 py-3 flex items-center justify-between text-sm"
                  >
                    <div class="w-0 flex-1 flex items-center">
                      <svg
                        class="flex-shrink-0 h-5 w-5 text-gray-400"
                        xmlns="http://www.w3.org/2000/svg"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke="currentColor"
                      >
                        <path
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"
                        />
                      </svg>
                      <span class="ml-2 flex-1 w-0 truncate">
                        {{ hour.time_from }} - {{ hour.time_to }}
                      </span>
                    </div>
                    <div class="ml-4 flex-shrink-0">
                      <a
                        href="#"
                        class="font-medium text-indigo-600 hover:text-indigo-500"
                      >
                        {{ hour.day_name }}
                      </a>
                    </div>
                  </li>
                </ul>
              </dd>
            </div>
          </dl>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      id: this.$route.params.id,
      destination: '',
      opening_hours: [],
      guides: [],
      currentTemp: '',
      minTemp: '',
      maxTemp: '',
      sunrise: '',
      sunset: '',
      pressure: '',
      humidity: '',
      wind: '',
      overcast: '',
      icon: '',
      name: '',
      lat: '',
      lon: '',
    }
  },

  beforeMount() {
    this.getWeather()
  },

  created() {
    fetch(
      `https://travvago-backend.herokuapp.com/api/v1/destination/detail?id=${this.id}`
    )
      .then((response) => response.json())
      .then((data) => {
        this.destination = data.results
        this.opening_hours = data.results.opening_hours
        this.guides = data.results.guides
      })
  },

  methods: {
    getWeather() {
      const lat = '-7.945660'
      const lng = '110.555190'
      const apiKey = '3bcd73cddf9df377bc3e2d51bcf915cf'
      fetch(
        `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lng}&APPID=${apiKey}`
      )
        .then((response) => response.json())
        .then((response) => {
          this.currentTemp = response.main.temp
          this.minTemp = response.main.temp_min
          this.maxTemp = response.main.temp_max
          this.pressure = response.main.pressure
          this.humidity = response.main.humidity + '%'
          this.wind = response.wind.speed + 'm/s'
          this.name = response.name
          this.lat = response.coord.lat
          this.lon = response.coord.lon
          this.overcast = response.weather[0].description
          this.icon = 'images/' + response.weather[0].icon.slice(0, 2) + '.svg'
          this.sunrise = new Date(response.sys.sunrise * 1000)
            .toLocaleTimeString('en-GB')
            .slice(0, 4)
          this.sunset = new Date(response.sys.sunset * 1000)
            .toLocaleTimeString('en-GB')
            .slice(0, 4)
        })
        .catch((error) => {
          console.log(error)
        })
    },
  },
}
</script>

<style></style>
