<template>
  <nav aria-label="Breadcrumb">
    <ol role="list" class="flex items-center bc">
      <li v-for="breadcrumb in product.breadcrumbs" :key="breadcrumb.id">
        <div class="flex items-center">
          <a
            :href="breadcrumb.href"
            class="mr-2 text-sm font-medium text-gray-900"
            >{{ breadcrumb.name }}</a
          >
          <svg
            width="16"
            height="20"
            viewBox="0 0 16 20"
            fill="currentColor"
            xmlns="http://www.w3.org/2000/svg"
            aria-hidden="true"
            class="h-5 w-4 text-gray-300"
          >
            <path d="M5.697 4.34L8.98 16.532h1.327L7.025 4.341H5.697z" />
          </svg>
        </div>
      </li>
      <li class="text-sm">
        <a
          :href="product.href"
          aria-current="page"
          class="font-medium text-gray-500 hover:text-gray-600"
          >{{ product.name }}</a
        >
      </li>
    </ol>
  </nav>

  <div class="bg-white">
    <div class="max-w-2xl py-16 px-4 sm:px-6 lg:max-w-7xl lg:px-8">
      <div class="lg:grid lg:grid-cols-2 lg:items-start lg:gap-x-8">
        <!-- Image gallery -->
        <TabGroup as="div" class="flex flex-col-reverse">
          <!-- Image selector -->
          <div
            class="mx-auto mt-6 hidden w-full max-w-2xl sm:block lg:max-w-none"
          >
            <TabList class="grid grid-cols-4 gap-6">
              <Tab
                v-for="image in product.images"
                :key="image.id"
                class="
                  relative
                  flex
                  h-24
                  cursor-pointer
                  items-center
                  justify-center
                  rounded-md
                  bg-white
                  text-sm
                  font-medium
                  uppercase
                  text-gray-900
                  hover:bg-gray-50
                  focus:outline-none
                  focus:ring
                  focus:ring-opacity-50
                  focus:ring-offset-4
                "
                v-slot="{ selected }"
              >
                <span class="sr-only"> {{ image.name }} </span>
                <span class="absolute inset-0 overflow-hidden rounded-md">
                  <img
                    :src="image.src"
                    alt=""
                    class="h-full w-full object-cover object-center"
                  />
                </span>
                <span
                  :class="[
                    selected ? 'ring-indigo-500' : 'ring-transparent',
                    'pointer-events-none absolute inset-0 rounded-md ring-2 ring-offset-2',
                  ]"
                  aria-hidden="true"
                />
              </Tab>
            </TabList>
          </div>

          <TabPanels class="aspect-w-1 aspect-h-1 w-full">
            <TabPanel v-for="image in product.images" :key="image.id">
              <img
                :src="image.src"
                :alt="image.alt"
                class="h-full w-full object-cover object-center sm:rounded-lg"
              />
            </TabPanel>
          </TabPanels>
        </TabGroup>

        <!-- Product info -->
        <div class="mt-10 px-4 sm:mt-16 sm:px-0 lg:mt-0">
          <h1 class="text-3xl font-bold tracking-tight text-gray-900">
            {{ product.name }}
          </h1>

          <div class="mt-3">
            <h2 class="sr-only">Product information</h2>
            <p class="text-m tracking-tight text-gray-900">
              Price : {{ product.price }}
            </p>
            <p class="text-m tracking-tight text-gray-900 mt-2">
              Min Pembelian : {{ product.order }}
            </p>
          </div>
          <div class="mt-6">
            <div
              class="space-y-6 text-sm font-bold text-gray-700"
              v-html="product.notes"
            />
          </div>

          <form class="mt-6">
            <!-- Colors -->
            <div>
              <h3 class="text-sm text-gray-600">Variant</h3>

              <Listbox v-model="selectedPerson">
                <div class="relative mt-1">
                  <ListboxButton
                    class="
                      relative
                      w-full
                      cursor-default
                      rounded-lg
                      bg-white
                      py-2
                      pl-3
                      pr-10
                      text-left
                      shadow-md
                      focus:outline-none
                      focus-visible:border-indigo-500
                      focus-visible:ring-2
                      focus-visible:ring-white
                      focus-visible:ring-opacity-75
                      focus-visible:ring-offset-2
                      focus-visible:ring-offset-orange-300
                      sm:text-sm
                    "
                  >
                    <span class="block truncate">{{
                      selectedPerson.name
                    }}</span>
                    <span
                      class="
                        pointer-events-none
                        absolute
                        inset-y-0
                        right-0
                        flex
                        items-center
                        pr-2
                      "
                    >
                      <ChevronUpDownIcon
                        class="h-5 w-5 text-gray-400"
                        aria-hidden="true"
                      />
                    </span>
                  </ListboxButton>

                  <transition
                    leave-active-class="transition duration-100 ease-in"
                    leave-from-class="opacity-100"
                    leave-to-class="opacity-0"
                  >
                    <ListboxOptions
                      class="
                        absolute
                        mt-1
                        max-h-60
                        w-full
                        overflow-auto
                        rounded-md
                        bg-white
                        py-1
                        text-base
                        shadow-lg
                        ring-1 ring-black ring-opacity-5
                        focus:outline-none
                        sm:text-sm
                      "
                    >
                      <ListboxOption
                        v-slot="{ active, selected }"
                        v-for="variants in variant"
                        :key="variants.name"
                        :value="variant"
                        as="template"
                      >
                        <li
                          :class="[
                            active
                              ? 'bg-amber-100 text-amber-900'
                              : 'text-gray-900',
                            'relative cursor-default select-none py-2 pl-10 pr-4',
                          ]"
                        >
                          <span
                            :class="[
                              selected ? 'font-medium' : 'font-normal',
                              'block truncate',
                            ]"
                            >{{ person.name }}</span
                          >
                          <span
                            v-if="selected"
                            class="
                              absolute
                              inset-y-0
                              left-0
                              flex
                              items-center
                              pl-3
                              text-amber-600
                            "
                          >
                            <CheckIcon class="h-5 w-5" aria-hidden="true" />
                          </span>
                        </li>
                      </ListboxOption>
                    </ListboxOptions>
                  </transition>
                </div>
              </Listbox>
            </div>

            <div class="sm:flex-col1 mt-10 flex">
              <button
                type="submit"
                class="
                  flex
                  max-w-xs
                  flex-1
                  items-center
                  justify-center
                  rounded-md
                  border border-transparent
                  bg-indigo-600
                  py-3
                  px-8
                  text-base
                  font-medium
                  text-white
                  hover:bg-indigo-700
                  focus:outline-none
                  focus:ring-2
                  focus:ring-indigo-500
                  focus:ring-offset-2
                  focus:ring-offset-gray-50
                  sm:w-full
                "
              >
                Add to bag
              </button>

              <button
                type="button"
                class="
                  ml-4
                  flex
                  items-center
                  justify-center
                  rounded-md
                  py-3
                  px-3
                  text-gray-400
                  hover:bg-gray-100 hover:text-gray-500
                "
              >
                <!-- <HeartIcon class="h-6 w-6 flex-shrink-0" aria-hidden="true" /> -->
                <span class="sr-only">Add to favorites</span>
              </button>
            </div>
          </form>
        </div>
        <section
          aria-labelledby="summary-heading"
          class="
            rounded-lg
            bg-gray-50
            px-4
            py-6
            sm:p-6
            lg:col-span-5 lg:mt-10 lg:p-8
            order
          "
        >
          <h2 id="summary-heading" class="text-lg font-medium text-gray-900">
            Order summary
          </h2>

          <dl class="mt-6 space-y-4">
            <div class="flex items-center justify-between">
              <dt class="text-sm text-gray-600">Subtotal</dt>
              <dd class="text-sm font-medium text-gray-900">$99.00</dd>
            </div>
            <div
              class="
                flex
                items-center
                justify-between
                border-t border-gray-200
                pt-4
              "
            >
              <dt class="flex items-center text-sm text-gray-600">
                <span>Shipping estimate</span>
                <a
                  href="#"
                  class="ml-2 flex-shrink-0 text-gray-400 hover:text-gray-500"
                >
                  <span class="sr-only"
                    >Learn more about how shipping is calculated</span
                  >
                  <QuestionMarkCircleIcon class="h-5 w-5" aria-hidden="true" />
                </a>
              </dt>
              <dd class="text-sm font-medium text-gray-900">$5.00</dd>
            </div>
            <div
              class="
                flex
                items-center
                justify-between
                border-t border-gray-200
                pt-4
              "
            >
              <dt class="flex text-sm text-gray-600">
                <span>Tax estimate</span>
                <a
                  href="#"
                  class="ml-2 flex-shrink-0 text-gray-400 hover:text-gray-500"
                >
                  <span class="sr-only"
                    >Learn more about how tax is calculated</span
                  >
                  <QuestionMarkCircleIcon class="h-5 w-5" aria-hidden="true" />
                </a>
              </dt>
              <dd class="text-sm font-medium text-gray-900">$8.32</dd>
            </div>
            <div
              class="
                flex
                items-center
                justify-between
                border-t border-gray-200
                pt-4
              "
            >
              <dt class="text-base font-medium text-gray-900">Order total</dt>
              <dd class="text-base font-medium text-gray-900">$112.32</dd>
            </div>
          </dl>

          <div class="mt-6">
            <button
              type="submit"
              class="
                w-full
                rounded-md
                border border-transparent
                bg-indigo-600
                py-3
                px-4
                text-base
                font-medium
                text-white
                shadow-sm
                hover:bg-indigo-700
                focus:outline-none
                focus:ring-2
                focus:ring-indigo-500
                focus:ring-offset-2
                focus:ring-offset-gray-50
              "
            >
              Checkout
            </button>
          </div>
        </section>
      </div>
    </div>
  </div>
</template>

<script setup>
import _ from "lodash";
import { ref } from "vue";
import {
  Disclosure,
  DisclosureButton,
  DisclosurePanel,
  Listbox,
  ListboxLabel,
  ListboxButton,
  ListboxOptions,
  ListboxOption,
  Tab,
  TabGroup,
  TabList,
  TabPanel,
  TabPanels,
} from "@headlessui/vue";
// import { CheckIcon, ChevronUpDownIcon } from "@heroicons/vue/20/solid";

const product = {
  name: "Assembling Building Block Sport Car",
  price: "Rp 278,099",
  order: "2",
  breadcrumbs: [
    { id: 1, name: "Home", href: "#" },
    { id: 2, name: "Product", href: "#" },
  ],
  images: [
    {
      id: 1,
      name: "Angled view",
      src: "https://cbu01.alicdn.com/img/ibank/O1CN01X4OYoI1H5qmQJYkpn_!!2211741960707-0-cib.jpg",
    },
    {
      id: 2,
      name: "View",
      src: "https://cbu01.alicdn.com/img/ibank/O1CN01P3XUpY1H5qmjdFi0c_!!2211741960707-0-cib.jpg",
    },
    {
      id: 3,
      name: "view",
      src: "https://cbu01.alicdn.com/img/ibank/O1CN01PAafr21H5qmDghl95_!!2211741960707-0-cib.jpg",
    },
    // More images...
  ],
  notes: `
    <p>Catatan: Minimum pemesanan yang tertera ditentukan oleh supplier dan tidak menjamin sudah memenuhi minimum pengiriman China - Indonesia (0.3 CBM/ 3 KG).</p>
  `,
};
const variant = [
  {
    name: "8600 Lambo Green Bull [1254 Particles] Static Edition",
  },
  {
    name: "8600 Rambo Green Bull [1254 Particles] Light Static Edition",
  },
  {
    name: "8600 Lambo Green Bull [1254 Particles] Remote Control Edition",
  },
  {
    name: "8604 Bugatti [1355 particles] static version",
  },
  {
    name: "8604 Bugatti [1355 Particles] Light Static Edition",
  },
];
const selectedPerson = ref(variant[0]);
</script>

<style lang="css" scoped>
.bc,
.order {
  margin-left: 150px;
  margin-top: 50px;
}
</style>