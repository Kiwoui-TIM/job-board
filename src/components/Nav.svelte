<script>
  import ProfileDropdown from '../components/ProfileDropdown.svelte';
  import NavLinks from '../components/NavLinks.svelte';
  import { auth } from '../firebase';
  import { authState } from 'rxfire/auth';

  // Current page
  export let segment;

  // Gets the current user from Firebase using an observable from rxfire. Makes
  // it super easy to get the authentication state in real time.
  let currentUser;
  const unsubscribe = authState(auth).subscribe(u => currentUser = u);

  // There's probably a more efficient way to do this, but uh... It works!
  // This function opens and closes the mobile nav menu
  async function toggleMobileNav() {
    const menu = document.querySelector('#mobile-nav');
    const openBtn = document.querySelector('#open-nav-icon');
    const closeBtn = document.querySelector('#close-nav-icon');

    if (menu.classList.contains('hidden')) {
      menu.classList.replace('hidden', 'block');
      openBtn.classList.replace('block', 'hidden');
      closeBtn.classList.replace('hidden', 'block');
    } else {
      menu.classList.replace('block', 'hidden');
      openBtn.classList.replace('hidden', 'block');
      closeBtn.classList.replace('block', 'hidden');
    }
  }
</script>

<nav class="bg-gray-800">
  <div class="px-2 sm:px-6 lg:px-8">
    <div class="relative flex items-center justify-between h-16">

      <!-- Mobile menu button -->
      <div class="absolute inset-y-0 left-0 flex items-center sm:hidden">
        <button on:click={toggleMobileNav} class="inline-flex items-center justify-center p-2 rounded-md text-gray-400 hover:text-white hover:bg-gray-700 focus:outline-none focus:bg-gray-700 focus:text-white transition duration-150 ease-in-out" aria-label="Main menu" aria-expanded="false">
          <!-- Heroicon name: menu -->
          <svg id="open-nav-icon" class="block h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
          </svg>
          <!-- Heroicon name: x -->
          <svg id="close-nav-icon" class="hidden h-6 w-6" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>
      </div>

      <!-- Desktop navigation -->
      <div class="flex-1 flex items-center justify-center sm:items-stretch sm:justify-start">
        <div class="flex-shrink-0">
          <img class="block lg:hidden h-8 w-auto" src="https://tailwindui.com/img/logos/v1/workflow-mark-on-dark.svg" alt="Workflow logo">
          <img class="hidden lg:block h-8 w-auto" src="https://tailwindui.com/img/logos/v1/workflow-logo-on-dark.svg" alt="Workflow logo">
        </div>
        <div class="hidden sm:block sm:ml-6">
          <div class="flex">
            <NavLinks { segment } />
          </div>
        </div>
      </div>

      <div class="absolute inset-y-0 right-0 flex items-center pr-2 sm:static sm:inset-auto sm:ml-6 sm:pr-0">
        <!-- Profile dropdown -->
        <!-- If the use is connected, show the dropdown, else show a login button -->
        {#if currentUser}
        <div class="ml-3 relative">
          <ProfileDropdown {...currentUser} />
        </div>
        {:else}
        <div class="ml-3 relative">
          <a href="/signin" class="inline-flex justify-center py-2 px-4 border border-transparent text-sm leading-5 font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-500 focus:outline-none focus:border-indigo-700 focus:shadow-outline-indigo active:bg-indigo-700 transition duration-150 ease-in-out">
            Connexion
          </a>
        </div>
        {/if}

        <!-- Post a job button, hidden in mobile layout -->
        <!-- This is commented out because it's not ready/implemented -->
        <!--
        <div class="ml-3 relative">
          <a href="#" class="inline-flex justify-center py-2 px-4 text-sm leading-5 font-medium text-white transition duration-150 ease-in-out hidden sm:block">
            Publier une offre
          </a>
        </div>
        -->
      </div>

    </div>
  </div>

  <!-- Mobile menu -->
  <div id="mobile-nav" class="hidden sm:hidden">
    <div class="px-2 pt-2 pb-3">
      <div class="mb-4 pb-2 border-b border-gray-400">
        <NavLinks {segment}/>
      </div>

      <!-- Post a job button in mobile menu -->
      <!-- This is commented out because it's not ready/implemented -->
      <!--
      <a href="#" class="block m-1 p-2 border border-transparent leading-5 rounded-md text-white bg-indigo-600 hover:bg-indigo-500 focus:outline-none focus:border-indigo-700 focus:shadow-outline-indigo active:bg-indigo-700 transition duration-150 ease-in-out">
        Publier une offre
      </a>
      -->
    </div>
  </div>
</nav>
