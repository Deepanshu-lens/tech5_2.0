<script lang="ts">
  import {
    ChevronDown,
    ChevronLeft,
    FacebookIcon,
    InstagramIcon,
    LinkedinIcon,
    TwitterIcon,
  } from "lucide-svelte";

  export let activeSec;
</script>

<div
  class="rounded-[12px] bg-[#F5F6F7] flex flex-col items-start w-screen relative"
>
  <button
    class="flex items-center justify-start text-black/[.7] pt-4"
    on:click={() => {
      activeSec = null;
    }}
  >
    <ChevronLeft class="h-[30px] w-[30px]" />
    <p class="text-lg font-semibold">Contact us</p>
  </button>
  <p class="text-3xl font-semibold text-start px-6 text-black mt-[10px]">
    Get in touch
  </p>
  <div
    class="w-full h-full overflow-y-scroll overflow-x-clip max-h-[calc(100vh-200px)] pb-10"
  >
    <div class="flex flex-col items-center w-full mx-auto mt-[30px] gap-[20px]">
      <span
        class="relative h-[48px] w-[335px] rounded-[8px] border-[1px] border-solid border-[rgb(145,158,171)]/[.32]"
      >
        <span
          class="absolute -top-3 left-6 text-sm font-medium text-[#637381] bg-[#f5f6f7]"
        >
          User type
        </span>
        <input
          class="h-full w-full leading-[16.8px] text-sm text-[#919eab] placeholder-[#919eab] bg-transparent px-6"
          placeholder="Select Type"
          autoComplete="off"
        />
        <ChevronDown
          class="text-[#637381] absolute right-4 top-1/2 -translate-y-1/2"
        />
      </span>
      <span
        class="relative h-[48px] w-[335px] rounded-[8px] border-[1px] border-solid border-[rgb(145,158,171)]/[.32]"
      >
        <span
          class="absolute -top-3 left-6 text-sm font-medium text-[#637381] bg-[#f5f6f7]"
        >
          Name
        </span>
        <input
          class="h-full w-full leading-[16.8px] text-sm text-[#919eab] placeholder-[#919eab] bg-transparent px-6"
          autoComplete="off"
          placeholder="Enter your name"
          type="text"
        />
      </span>
      <span
        class="relative h-[48px] w-[335px] rounded-[8px] border-[1px] border-solid border-[rgb(145,158,171)]/[.32]"
      >
        <span
          class="absolute -top-3 left-6 text-sm font-medium text-[#637381] bg-[#f5f6f7]"
        >
          Phone no.
        </span>
        <input
          class="h-full w-full leading-[16.8px] text-sm text-[#919eab] placeholder-[#919eab] bg-transparent px-6"
          placeholder="Enter"
          type="number"
          autoComplete="off"
        />
      </span>
      <span
        class="relative h-[48px] w-[335px] rounded-[8px] border-[1px] border-solid border-[rgb(145,158,171)]/[.32]"
      >
        <span
          class="absolute -top-3 left-6 text-sm font-medium text-[#637381] bg-[#f5f6f7]"
        >
          Email
        </span>
        <input
          type="email"
          class="h-full w-full leading-[16.8px] text-sm text-[#919eab] placeholder-[#919eab] bg-transparent px-6"
          placeholder="Enter email id"
        />
      </span>
      <span
        class="relative h-[72px] w-[335px] rounded-[8px] border-[1px] border-solid border-[rgb(145,158,171)]/[.32]"
      >
        <span
          class="absolute -top-3 left-6 text-sm font-medium text-[#637381] bg-[#f5f6f7]"
        >
          Message
        </span>
        <textarea
          class=" hide-scrollbar h-full w-full leading-[16.8px] text-sm text-[#919eab] placeholder-[#919eab] bg-transparent px-6 py-6 focus:outline-none"
          placeholder="Enter Text"
          name="message"
        />
      </span>
      <button class="px-10 rounded-lg h-[40px] bg-[#015a62] text-white">
        Send message
      </button>
    </div>
    <div
      class="h-full w-full mt-4 bg-contact-bg bg-cover px-6 flex flex-row items-end py-8 text-white"
    >
      <span class="flex flex-col items-start">
        <p class="mb-6">SITEMAP</p>
        <p>Home</p>
        <p>Products</p>
        <p>Blogs</p>
        <p>About us</p>
      </span>
      <span class="flex flex-row items-center gap-2 ml-auto">
        <FacebookIcon class="h-[26px] w-[26px]" />
        <InstagramIcon class="h-[26px] w-[26px]" />
        <LinkedinIcon class="h-[26px] w-[26px]" />
        <TwitterIcon class="h-[26px] w-[26px]" />
      </span>
    </div>
  </div>
</div>
