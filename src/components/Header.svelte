<script>
  export let tabs = [
    { name: "Showcase", link: "#introPage" },
    { name: "About me", link: "#about" },
    { name: "Contact", link: "#contact" }
  ];

  let scrolled = false;
  let showHeader = false;
  let mobileOpen = false;

  import { onMount } from "svelte";

  onMount(() => {
    showHeader = true;

    const handleScroll = () => {
      scrolled = window.scrollY > 20;
    };

    window.addEventListener("scroll", handleScroll);
    handleScroll();

    return () => window.removeEventListener("scroll", handleScroll);
  });

  function toggleMobile() {
    mobileOpen = !mobileOpen;
  }
</script>

<style>
  header {
    backdrop-filter: blur(12px);
    -webkit-backdrop-filter: blur(12px);
  }

  .fade-down {
    opacity: 0;
    transform: translateY(-20px);
    transition: all 0.6s ease;
  }

  .fade-down.show {
    opacity: 1;
    transform: translateY(0);
  }

  .nav-link {
    position: relative;
    display: inline-block;
    transition: all 0.3s ease;
  }

  .nav-link::after {
    content: "";
    position: absolute;
    width: 0;
    height: 2px;
    bottom: -4px;
    left: 0;
    background: linear-gradient(to right, #8b5cf6, #6366f1);
    transition: width 0.3s ease;
  }

  .nav-link:hover::after {
    width: 100%;
  }

  .nav-link:hover {
    color: #a78bfa;
    transform: scale(1.05);
  }

  .brand {
    opacity: 0;
    transform: translateX(-30px);
    transition: all 0.8s ease-out;
  }

  .brand.show {
    opacity: 1;
    transform: translateX(0);
  }

  html {
    scroll-behavior: smooth;
  }

  .mobile-menu {
    transition: max-height 0.3s ease, opacity 0.3s ease;
    overflow: hidden;
  }

  /* Mobile-specific styles */
  .mobile-nav {
    display: none;
    background-color: rgba(31, 41, 55, 0.9); /* Dark background */
    position: absolute;
    top: 100%;
    left: 0;
    right: 0;
    padding-top: 1rem;
    text-align: center;
    z-index: 100;
  }

  .mobile-nav.open {
    display: block;
  }

  .mobile-nav a {
    display: block;
    padding: 0.5rem;
    font-size: 1.1rem;
    font-weight: 500;
    color: white;
    transition: color 0.3s ease;
  }

  .mobile-nav a:hover {
    color: #a78bfa;
  }

  /* Mobile button adjustment */
  .mobile-menu-button {
    position: absolute;
    top: 1rem; /* position the button at the top */
    right: 1rem; /* position the button to the right */
    z-index: 101;
  }

  @media (min-width: 768px) {
    .mobile-menu-button {
      display: none;
    }
    .mobile-nav {
      display: none;
    }
    .desktop-nav {
      display: flex;
    }
  }

  @media (max-width: 767px) {
    .desktop-nav {
      display: none;
    }
  }
</style>

<header
  class="w-full sticky top-0 z-[50] px-0 flex items-center justify-between border-b border-solid py-6 bg-slate-950/70 border-violet-950 transition-shadow duration-300 {scrolled ? 'shadow-md' : ''}"
>
  <div class="w-full px-3 flex items-center justify-between fade-down {showHeader ? 'show' : ''}">
    <h1 class="font-medium brand {showHeader ? 'show' : ''}">
      <b class="font-bold poppins hover:scale-105 transition-transform duration-300">
        Devbyvann
      </b>
    </h1>

    <!-- Desktop Nav -->
    <nav class="desktop-nav space-x-8 text-sm font-medium">
      {#each tabs as tab}
        <a href={tab.link} class="nav-link">{tab.name}</a>
      {/each}
    </nav>

    <!-- Mobile Menu Button -->
    <button
      class="mobile-menu-button focus:outline-none"
      on:click={toggleMobile}
    >
      <svg
        class="w-6 h-6 text-white"
        fill="none"
        stroke="currentColor"
        viewBox="0 0 24 24"
        xmlns="http://www.w3.org/2000/svg"
      >
        {#if !mobileOpen}
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M4 6h16M4 12h16M4 18h16"
          />
        {:else}
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M6 18L18 6M6 6l12 12"
          />
        {/if}
      </svg>
    </button>
  </div>

  <!-- Mobile Nav -->
  <div class="mobile-nav {mobileOpen ? 'open' : ''}">
    {#each tabs as tab}
      <a
        href={tab.link}
        class="nav-link"
        on:click={() => (mobileOpen = false)}
      >
        {tab.name}
      </a>
    {/each}
  </div>
</header>
