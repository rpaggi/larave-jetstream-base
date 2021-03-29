<template>
    <div>
        <div class="min-h-screen bg-gray-100">

            <nav class="bg-white border-b border-gray-300 shadow-lg z-10">
                <!-- Primary Navigation Menu -->
                <div class="max-w-12xl mx-auto px-4 sm:px-6 lg:px-8">
                    <div class="flex justify-between h-28">
                        <div class="flex">
                        </div>
                        <div class="flex">
                            <!-- Logo -->
                            <div class="flex-shrink-0 flex items-center">
                                <inertia-link :href="route('dashboard')">
                                    <h1 class="text-2xl text-gray-500 hover:text-green-400">Laravel</h1>
                                </inertia-link>
                            </div>
                        </div>

                        <div class="hidden sm:flex sm:items-center sm:ml-6">
                            <!-- Settings Dropdown -->
                            <div class="ml-3 relative">
                                <jet-dropdown align="right" width="48">
                                    <template #trigger>
                                        <button v-if="$page.props.jetstream.managesProfilePhotos" class="flex text-sm border-2 border-transparent rounded-full focus:outline-none focus:border-gray-300 transition duration-150 ease-in-out">
                                            <img class="h-8 w-8 rounded-full object-cover" :src="$page.props.user.profile_photo_url" :alt="$page.props.user.name" :title="$page.props.user.name" />
                                        </button>

                                        <span v-else class="inline-flex rounded-md">
                                            <button type="button" class="inline-flex items-center px-3 py-2 border border-transparent text-sm leading-4 font-medium rounded-md text-gray-500 bg-white hover:text-gray-700 focus:outline-none transition ease-in-out duration-150">
                                                {{ $page.props.user.name }}

                                                <svg class="ml-2 -mr-0.5 h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                                                    <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd" />
                                                </svg>
                                            </button>
                                        </span>
                                    </template>

                                    <template #content>
                                        <jet-dropdown-link :href="route('profile.show')">
                                            Minha conta
                                        </jet-dropdown-link>

                                        <div class="border-t border-gray-100"></div>

                                        <!-- Authentication -->
                                        <form @submit.prevent="logout">
                                            <jet-dropdown-link as="button">
                                                Deslogar
                                            </jet-dropdown-link>
                                        </form>
                                    </template>
                                </jet-dropdown>
                            </div>
                        </div>

                        <!-- Hamburger -->
                        <div class="-mr-2 flex items-center sm:hidden">
                            <button @click="showingNavigationDropdown = ! showingNavigationDropdown" class="inline-flex items-center justify-center p-2 rounded-md text-gray-400 hover:text-gray-500 hover:bg-gray-100 focus:outline-none focus:bg-gray-100 focus:text-gray-500 transition duration-150 ease-in-out">
                                <svg class="h-6 w-6" stroke="currentColor" fill="none" viewBox="0 0 24 24">
                                    <path :class="{'hidden': showingNavigationDropdown, 'inline-flex': ! showingNavigationDropdown }" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
                                    <path :class="{'hidden': ! showingNavigationDropdown, 'inline-flex': showingNavigationDropdown }" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                                </svg>
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Responsive Navigation Menu -->
                <div :class="{'block': showingNavigationDropdown, 'hidden': ! showingNavigationDropdown}" class="sm:hidden">

                    <!-- Responsive Settings Options -->
                    <div class="pt-4 pb-1 border-t border-gray-200">
                        <div class="flex items-center px-4">
                            <div v-if="$page.props.jetstream.managesProfilePhotos" class="flex-shrink-0 mr-3" >
                                <img class="h-10 w-10 rounded-full object-cover" :src="$page.props.user.profile_photo_url" :alt="$page.props.user.name" />
                            </div>

                            <div>
                                <div class="font-medium text-base text-gray-800">{{ $page.props.user.name }}</div>
                                <div class="font-medium text-sm text-gray-500">{{ $page.props.user.email }}</div>
                            </div>
                        </div>

                        <div class="mt-3 space-y-1">
                            <jet-responsive-nav-link 
                                v-for="m in menu" 
                                :key="m.name"
                                :href="route(m.to)" 
                                :active="isActive(m.to)"
                            >
                                {{m.label}}
                            </jet-responsive-nav-link>
                        </div>

                        <div class="mt-3 space-y-1">
                            <jet-responsive-nav-link :href="route('profile.show')" :active="route().current('profile.show')">
                                Meu Perfil
                            </jet-responsive-nav-link>

                            <!-- Authentication -->
                            <form method="POST" @submit.prevent="logout">
                                <jet-responsive-nav-link as="button">
                                    Deslogar
                                </jet-responsive-nav-link>
                            </form>
                        </div>
                    </div>
                </div>
            </nav>

            <!-- Page Content -->
            <main class="grid grid-cols-12">
                <div class="hidden sm:block col-span-1 py-5 bg-white border-b border-gray-100 h-screen shadow-menu-right z-10">
                    <ul>
                        <inertia-link v-for="m in menu" :key="m.name" :href="route(m.to)">
                            <li 
                                class="flex flex-col justify-center text-4xl text-gray-400 py-5 hover:text-green-400 cursor-pointer border-l-8 border-white" 
                                :class="{
                                    'text-green-400 border-green-400':isActive(m.to)
                                }"
                            >
                                <i class="text-center" :class="m.icon"></i>
                                <span class="text-sm text-center pt-1">{{m.label}}</span>
                            </li>
                        </inertia-link>
                    </ul>
                </div>  
                <div class="col-span-full sm:col-span-11 py-5 px-8 bg-gray-100">
                    <slot></slot>
                </div>  
            </main>
        </div>
    </div>
</template>

<script>
    // route('profile.show')
    import JetDropdown from '@/Jetstream/Dropdown'
    import JetDropdownLink from '@/Jetstream/DropdownLink'
    import JetNavLink from '@/Jetstream/NavLink'
    import JetResponsiveNavLink from '@/Jetstream/ResponsiveNavLink'

    export default {
        components: {
            JetDropdown,
            JetDropdownLink,
            JetNavLink,
            JetResponsiveNavLink,
        },

        data() {
            return {
                showingNavigationDropdown: false,
                menu:[
                    {
                        name:'home',
                        label:'Home',
                        icon:'iconsminds-home',
                        to:'dashboard'
                    }
                ]
            }
        },

        methods: {
            isActive(url){
                let current = window.location.pathname
                let path = route(url).replace(window.location.origin, '')
                let active = false

                if(path && current.includes(path)) active = true;

                if(!path){
                    path = '/'
                }

                if(path == current) active = true

                return active
            },

            logout() {
                this.$inertia.post(route('logout'));
            },
        }
    }
</script>
