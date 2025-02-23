<script setup>
import { Head, Link, useForm } from '@inertiajs/vue3';
import ApplicationLogo from '@/Components/ApplicationLogo.vue';
import Dropdown from '@/Components/Dropdown.vue';
import DropdownLink from '@/Components/DropdownLink.vue';
import NavLink from '@/Components/NavLink.vue';
import ResponsiveNavLink from '@/Components/ResponsiveNavLink.vue';
import { ref } from 'vue';
import { defineProps } from 'vue';



const props = defineProps({
    products:{
        type: Array,
        default: () => [],
    },
    category:{
        type: Array,
        default: () => [],
    }
});

const showingNavigationDropdown = ref(false);
    
const scrollToCategory = (categoryName) => {
    const section = document.getElementById(categoryName);
    if (section) {
        section.scrollIntoView({ behavior: 'smooth' });
    }
};


const searchQuery = ref('');
const searchResults = ref([]);

const searchProducts = async () => {
    try {
        const query = searchQuery.value.toLowerCase(); // Convert the search query to lowercase
        const response = await fetch('/search'); // Fetch all products or relevant dataset
        
        if (response.ok) {
            const products = await response.json();
            // Filter results to make the search case-insensitive
            searchResults.value = products.filter(product =>
                product.name.toLowerCase().includes(query) // Compare lowercase names
            );
        } else {
            console.error('Failed to fetch search results');
        }
    } catch (error) {
        console.error('Error searching products:', error);
    }
};



const form = useForm({
    id: 0,
    qty: 1,
});

const addToCart = (p_id) => {
    form.id = p_id;
    form.post('/cart/add');
    alert("added to cart successfully.");
    alert("you can edit you quantity in your cart.");   
};

</script>

<template>
    <Head title="COFI - Carry the Merry" />
    
    <!-- Main Layout -->
    <div class="min-h-screen bg-[#2D5522] ">
        <nav class="fixed bg-[#2D5522] top-0 left-0 right-0 border-gray-100 text-white">
            <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
                <div class="flex h-16 justify-between items-center">
                    <div class="flex ">
                        <!-- Logo -->
                        <div class="flex shrink-0 items-center ">
                            <Link :href="route('dashboard')">
                                <ApplicationLogo class="block h-9 w-auto fill-current text-gray-800" />
                            </Link>
                            <NavLink :href="route('dashboard')" class="ml-2 text-xl font-bold text-gray-900">COFI</NavLink>
                        </div>

                        <!-- Navigation Links -->
                        <div class="hidden space-x-8 sm:-my-px sm:ms-10 sm:flex">
                            <NavLink :href="route('dashboard')">HOME</NavLink>
                            <div class="relative group border-b-2 border-transparent hover:border-gray-300">
                              <!-- Trigger -->
                              <span class="inline-flex rounded-md">
                                  <button type="button" class="mt-2 inline-flex items-center px-1 border-t-2 border-transparent text-sm font-medium leading-5 text-white hover:text-gray-400 focus:outline-none focus:text-gray-300 transition duration-150 ease-in-out">
                                      MENU
                                  </button>
                              </span>

                              <!-- Dropdown Content -->
                              <div
                                  class="absolute left-0 z-50 w-48 rounded-md bg-white shadow-lg hidden group-hover:block">
                                  <DropdownLink @click="scrollToCategory(category.name)">Holiday Limited Edition</DropdownLink>
                                  <DropdownLink href="#Coffee_Beverage">Coffee Beverage</DropdownLink>
                                  <DropdownLink href="#Foods & Snacks">Foods & Snacks</DropdownLink>
                                  <DropdownLink href="#premium">Coffee Beans & Ground Coffee</DropdownLink>
                              </div>
                            </div>
                            <NavLink :href="route('contact')">CONTACT US</NavLink>
                        </div>
                    </div>

                    <div class="hidden sm:ms-6 sm:flex sm:items-center">
                        <NavLink :href="route('transactions.page')">
                                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-9 h-9 text-right me-4">
                                    <path fill-rule="evenodd" d="M4.125 3C3.089 3 2.25 3.84 2.25 4.875V18a3 3 0 0 0 3 3h15a3 3 0 0 1-3-3V4.875C17.25 3.839 16.41 3 15.375 3H4.125ZM12 9.75a.75.75 0 0 0 0 1.5h1.5a.75.75 0 0 0 0-1.5H12Zm-.75-2.25a.75.75 0 0 1 .75-.75h1.5a.75.75 0 0 1 0 1.5H12a.75.75 0 0 1-.75-.75ZM6 12.75a.75.75 0 0 0 0 1.5h7.5a.75.75 0 0 0 0-1.5H6Zm-.75 3.75a.75.75 0 0 1 .75-.75h7.5a.75.75 0 0 1 0 1.5H6a.75.75 0 0 1-.75-.75ZM6 6.75a.75.75 0 0 0-.75.75v3c0 .414.336.75.75.75h3a.75.75 0 0 0 .75-.75v-3A.75.75 0 0 0 9 6.75H6Z" clip-rule="evenodd" />
                                    <path d="M18.75 6.75h1.875c.621 0 1.125.504 1.125 1.125V18a1.5 1.5 0 0 1-3 0V6.75Z" />
                                </svg>
                        </NavLink>  
                      <NavLink :href="route('cart')">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-9 h-9 text-right">
                            <path d="M2.25 2.25a.75.75 0 0 0 0 1.5h1.386c.17 0 .318.114.362.278l2.558 9.592a3.752 3.752 0 0 0-2.806 3.63c0 .414.336.75.75.75h15.75a.75.75 0 0 0 0-1.5H5.378A2.25 2.25 0 0 1 7.5 15h11.218a.75.75 0 0 0 .674-.421 60.358 60.358 0 0 0 2.96-7.228.75.75 0 0 0-.525-.965A60.864 60.864 0 0 0 5.68 4.509l-.232-.867A1.875 1.875 0 0 0 3.636 2.25H2.25ZM3.75 20.25a1.5 1.5 0 1 1 3 0 1.5 1.5 0 0 1-3 0ZM16.5 20.25a1.5 1.5 0 1 1 3 0 1.5 1.5 0 0 1-3 0Z" />
                        </svg>
                    </NavLink>
                        <!-- Settings Dropdown -->
                        <div class="relative ms-3">
                            <Dropdown align="right" width="48">
                                <template #trigger>
                                    <span class="inline-flex rounded-md">
                                        <button type="button" class="inline-flex items-center rounded-md border border-transparent bg-white px-3 py-2 text-sm font-medium leading-4 text-gray-500 transition duration-150 ease-in-out hover:text-gray-700 focus:outline-none">
                                            {{ $page.props.auth.user.name }}
                                            <svg class="-me-0.5 ms-2 h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                                                <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd" />
                                            </svg>
                                        </button>
                                    </span>
                                </template>

                                <template #content>
                                    <DropdownLink :href="route('profile.edit')">Profile</DropdownLink>
                                    <DropdownLink :href="route('logout')" method="post" as="button">Log Out</DropdownLink>
                                </template>
                            </Dropdown>
                        </div>
                    </div>

                    <!-- Hamburger -->
                    <div class="-me-2 flex items-center sm:hidden">
                        <button @click="showingNavigationDropdown = !showingNavigationDropdown" class="inline-flex items-center justify-center rounded-md p-2 text-gray-400 transition duration-150 ease-in-out hover:bg-gray-100 hover:text-gray-500 focus:bg-gray-100 focus:text-gray-500 focus:outline-none">
                            <svg class="h-6 w-6" stroke="currentColor" fill="none" viewBox="0 0 24 24">
                                <path :class="{ hidden: showingNavigationDropdown, 'inline-flex': !showingNavigationDropdown }" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
                                <path :class="{ hidden: !showingNavigationDropdown, 'inline-flex': showingNavigationDropdown }" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                            </svg>
                        </button>
                    </div>
                </div>
            </div>

            <!-- Responsive Navigation Menu -->
            <div :class="{ block: showingNavigationDropdown, hidden: !showingNavigationDropdown }" class="sm:hidden">
                <div class="space-y-1 pb-3 pt-2">
                    <ResponsiveNavLink :href="route('dashboard')" :active="route().current('dashboard')">HOME</ResponsiveNavLink>
                    <ResponsiveNavLink :href="route('menu')" :active="route().current('menu')">MENU</ResponsiveNavLink>
                    <ResponsiveNavLink :href="route('contact')" :active="route().current('contact')">CONTACT US</ResponsiveNavLink>
                    <ResponsiveNavLink :href="route('cart')" :active="route().current('cart')">MY CART</ResponsiveNavLink>
                    <ResponsiveNavLink :href="route('transactions.page')" :active="route().current('transactions.page')">TRANSACITIONS</ResponsiveNavLink>
                </div>

                <div class="border-t border-gray-200 pb-1 pt-4">
                    <div class="px-4">
                        <div class="text-base font-medium text-gray-800">{{ $page.props.auth.user.name }}</div>
                        <div class="text-sm font-medium text-gray-500">{{ $page.props.auth.user.email }}</div>
                    </div>

                    <div class="mt-3 space-y-1">
                        <ResponsiveNavLink :href="route('profile.edit')">Profile</ResponsiveNavLink>
                        <ResponsiveNavLink :href="route('logout')" method="post" as="button">Log Out</ResponsiveNavLink>
                    </div>
                </div>
            </div>
        </nav>

        <div class="min-h-screen bg-gray-50 pt-5 mt-20">

          <!-- search button -->
      <!-- Search button -->
      <div class="md:text-right me-20">
        <div class="input-container inline-flex items-center rounded-lg overflow-hidden">
          <input 
            v-model="searchQuery"
            @keyup.enter="searchProducts"
            type="text" 
            class="ms-5 py-2 px-4 w-64 text-black placeholder-gray-400 bg-transparent transition-all duration-300" 
            placeholder="Enter Product">
          <button 
            @click="searchProducts"
            class="text-white font-medium py-2 px-4 transition-all bg-[#2D5522]">
            Search
          </button>
        </div>
      </div>

      <!-- Search Results -->
      <section v-if="searchResults.length" class="mt-6 px-6">
        <h3 class="text-xl font-semibold">Search Results</h3>
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
          <div 
            v-for="product in searchResults" 
            :key="product.id" 
            class="rounded-lg p-4 text-center shadow-md bg-gray-100">
            <img :src="`/storage/${product.image}`" :alt="product.name" class="h-32 mx-auto mb-2">
            <h3 class="text-lg font-semibold">{{ product.name }}</h3>
            <p class="text-gray-500">₱{{ product.price }}</p>
            <button @click="openModal(product)" class="mt-2 w-full py-1 bg-green-600 text-white font-semibold rounded-lg hover:bg-green-700">
              Add to Cart
            </button>
          </div>
        </div>
      </section>

      <!-- Menu Categories -->
      <section class="px-6 py-10 space-y-10">
        <div v-for="category in props.category" 
          :key="category.id" 
          :id="category.name" 
          class="md:pt-20">
          <h2 class="text-3xl font-bold text-green-600">{{category.id === 1 ? 'Holiday Limited Edition' : category.name }}</h2>
          <div class="mt-6 grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
            <div 
              v-for="product in products.filter(product => product.category_id === category.id && product.availability ===   true)" 
              :key="product.id" 
              :class="{
                'bg-gray-100': product.category_id !== 1,
                'bg-cover bg-center': product.category_id === 1
              }"
              :style="product.category_id === 1 ? { backgroundImage: 'url(left.gif)' } : {}"
              class="rounded-lg p-4 text-center shadow-md"
            >
              <img :src="`/storage/${product.image}`" :alt="product.name" class="h-32 mx-auto mb-2">
              <h3 class="text-lg font-semibold">{{ product.name }}</h3>
              <p class="text-gray-500">₱{{ product.price }}</p>
              <button @click="addToCart(product.id)" class="mt-2 w-full py-1 bg-green-600 text-white font-semibold rounded-lg hover:bg-green-700">
                Add to Cart
              </button>
            </div>
          </div>
        </div>
      </section>

        
            <!-- Footer -->
            <footer class="bg-gray-800 text-white py-8 mt-12">
                <div class="container mx-auto px-4">
                    <div class="flex flex-wrap justify-between">
                        <div class="w-full sm:w-1/2 md:w-1/3 mb-6 md:mb-0">
                            <h3 class="text-xl font-bold mb-2">COFI</h3>
                            <p class="text-gray-400">Brewing happiness, one cup at a time.</p>
                        </div>
                        <div class="w-full sm:w-1/2 md:w-1/3 mb-6 md:mb-0">
                            <h3 class="text-xl font-bold mb-2">Quick Links</h3>
                            <div class="flex flex-col space-y-4">
                              <NavLink :href="route('dashboard')" :active="route().current('dashboard')" class="text-gray-400 hover:text-white">HOME</NavLink>
                              <NavLink :href="route('menu')" :active="route().current('menu')"  class="text-gray-400 hover:text-white">MENU</NavLink>
                              <NavLink :href="route('contact')" :active="route().current('contact')"  class="text-gray-400 hover:text-white">CONTACT US</NavLink>
                              <NavLink :href="route('cart')" :active="route().current('cart')" class="text-gray-400 hover:text-white">MY CART</NavLink>
                            </div>
                          </div>
                          
                        <div class="w-full sm:w-1/2 md:w-1/3">
                            <h3 class="text-xl font-bold mb-2">Connect With Us</h3>
                            <div class="flex space-x-4">
                              <!--FACEBOOK-->
                                <a href="#" class="text-gray-400 hover:text-white">
                                    <svg class="h-6 w-6" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true">
                                        <path fill-rule="evenodd" d="M22  12c0-5.523-4.477-10-10-10S2 6.477 2 12c0 4.991 3.657 9.128 8.438 9.878v-6.987h-2.54V12h2.54V9.797c0-2.506 1.492-3.89 3.777-3.89 1.094 0 2.238.195 2.238.195v2.46h-1.26c-1.243 0-1.63.771-1.63 1.562V12h2.773l-.443 2.89h-2.33v6.988C18.343 21.128 22 16.991 22 12z" clip-rule="evenodd" />
                                    </svg>
                                </a>
                                <!--INSTAGRAM-->
                                <a href="#" class="text-gray-400 hover:text-white">
                                    <svg class="h-6 w-6" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true">
                                        <path fill-rule="evenodd" d="M12.315 2c2.43 0 2.784.013 3.808.06 1.064.049 1.791.218 2.427.465a4.902 4.902 0 011.772 1.153 4.902 4.902 0 011.153 1.772c.247.636.416 1.363.465 2.427.048 1.067.06 1.407.06 4.123v.08c0 2.643-.012 2.987-.06 4.043-.049 1.064-.218 1.791-.465 2.427a4.902 4.902 0 01-1.153 1.772 4.902 4.902 0 01-1.772 1.153c-.636.247-1.363.416-2.427.465-1.067.048-1.407.06-4.123.06h-.08c-2.643 0-2.987-.012-4.043-.06-1.064-.049-1.791-.218-2.427-.465a4.902 4.902 0 01-1.772-1.153 4.902 4.902 0 01-1.153-1.772c-.247-.636-.416-1.363-.465-2.427-.047-1.024-.06-1.379-.06-3.808v-.63c0-2.43.013-2.784.06-3.808.049-1.064.218-1.791.465-2.427a4.902 4.902 0 011.153-1.772A4.902 4.902 0 015.45 2.525c.636-.247 1.363-.416 2.427-.465C8.901 2.013 9.256 2 11.685 2h.63zm-.081 1.802h-.468c-2.456 0-2.784.011-3.807.058-.975.045-1.504.207-1.857.344-.467.182-.8.398-1.15.748-.35.35-.566.683-.748 1.15-.137.353-.3.882-.344 1.857-.047 1.023-.058 1.351-.058 3.807v.468c0 2.456.011 2.784.058 3.807.045.975.207 1.504.344 1.857.182.466.399.8.748 1.15.35.35.683.566 1.15.748.353.137.882.3 1.857.344 1.054.048 1.37.058 4.041.058h.08c2.597 0 2.917-.01 3.96-.058.976-.045 1.505-.207 1.858-.344.466-.182.8-.398 1.15-.748.35-.35.566-.683.748-1.15.137-.353.3-.882.344-1.857.048-1.055.058-1.37.058-4.041v-.08c0-2.597-.01-2.917-.058-3.96-.045-.976-.207-1.505-.344-1.858a3.097 3.097 0 00-.748-1.15 3.098 3.098 0 00-1.15-.748c-.353-.137-.882-.3-1.857-.344-1.023-.047-1.351-.058-3.807-.058zM12 6.865a5.135 5.135 0 110 10.27 5.135 5.135 0 010-10.27zm0 1.802a3.333 3.333 0 100 6.666 3.333 3.333 0 000-6.666zm5.338-3.205a1.2 1.2 0 110 2.4 1.2 1.2 0 010-2.4z" clip-rule="evenodd" />
                                    </svg>
                                </a>
                                <!--TWITTER-->
                                <a href="#" class="text-gray-400 hover:text-white">
                                    <svg class="h-6 w-6" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true">
                                        <path d="M8.29 20.251c7.547 0 11.675-6.253 11.675-11.675 0-.178 0-.355-.012-.53A8.348 8.348 0 0022 5.92a8.19 8.19 0 01-2.357.646 4.118 4.118 0 001.804-2.27 8.224 8.224 0 01-2.605.996 4.107 4.107 0 00-6.993 3.743 11.65 11.65 0 01-8.457-4.287 4.106 4.106 0 001.27 5.477A4.072 4.072 0 012.8 9.713v.052a4.105 4.105 0 003.292 4.022 4.095 4.095 0 01-1.853.07 4.108 4.108 0 003.834 2.85A8.233 8.233 0 012 18.407a11.616 11.616 0 006.29 1.84" />
                                    </svg>
                                </a>
                            </div>
                        </div>
                    </div>
                    <div class="mt-8 text-center text-gray-400 text-sm">
                        <p>© 2024 COFI. All rights reserved.</p>
                        <p>Developed by Rocales | Puspus | Prisco.</p>
                    </div>
                </div>
            </footer>
          </div>
    </div>
</template>

