<script lang="ts">
  import { onMount } from "svelte";

  // Navigation links
  const navLinks = [
    { name: "Live", href: "/" },
    { name: "Playback", href: "/playback" },
    { name: "Events", href: "/events" },
    { name: "Gallery", href: "/gallery" },
    { name: "Configuration", href: "/configuration" },
    { name: "Reports", href: "/reports" },
  ];

  // Reactive state for menu toggle
  let isMenuOpen = false;

  // Reactive value for the current pathname
  let currentPath = window.location.pathname;

  // Function to check if the link is active
  let isActive: (href: string) => boolean;
  $: isActive = (href: string) => {
    return currentPath === href;
  };
</script>

<!-- 
<header
    class="h-10 md:h-16 backdrop-blur-lg bg-white/80 shadow-md px-4 md:px-4 lg:px-16 text-gray-600"
> -->
<div class="mx-auto w-full flex justify-between items-center h-full">
  <div class="flex items-center space-x-3">
    <!-- <img src="/images/green.svg" alt="LENS View Logo" class="h-8" /> -->
  </div>

  <!-- Desktop navigation -->
  <nav class="hidden md:flex space-x-4">
    {#each navLinks as link}
      <a
        href={link.href}
        on:click={() => (currentPath = link.href)}
        class="text-sm dark:text-[#015a62] transition-all px-3 py-2 rounded-md
                    {isActive(link.href)
          ? 'font-medium bg-gray-200 text-black'
          : 'font-normal hover:bg-gray-100 hover:text-black'}"
      >
        {link.name}
      </a>
    {/each}
  </nav>

  <!-- Hamburger for mobile view -->
  <div class="md:hidden">
    <button
      on:click={() => (isMenuOpen = !isMenuOpen)}
      class="focus:outline-none"
    >
      <svg
        fill="none"
        stroke="currentColor"
        stroke-linecap="round"
        stroke-linejoin="round"
        stroke-width="1.5"
        shape-rendering="geometricPrecision"
        viewBox="0 0 24 24"
        height="1.125rem"
        width="1.125rem"
        style="color: currentcolor;"
      >
        <path d="M3 12h18M3 6h18M3 18h18" />
      </svg>
    </button>
  </div>

  <div class="hidden md:flex items-center space-x-2">
    <span class="text-sm font-medium">Debayan Deb</span>
  </div>
</div>

<!-- Mobile menu dropdown -->
{#if isMenuOpen}
  <div class="md:hidden bg-white/80 shadow-md backdrop-blur-lg">
    <nav class="flex flex-col space-y-2 px-4 py-2">
      {#each navLinks as link}
        <a
          href={link.href}
          class="text-sm transition-all px-3 py-2 rounded-md dark:text-[#015a62]
              {isActive(link.href)
            ? 'font-medium bg-gray-200 text-black'
            : 'font-normal text-gray-600 hover:bg-gray-300 hover:text-black'}"
        >
          {link.name}
        </a>
      {/each}
    </nav>
  </div>
{/if}
<!-- </header> -->
