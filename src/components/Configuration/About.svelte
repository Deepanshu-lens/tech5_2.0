<script lang="ts">
  import { X } from "lucide-svelte";

  export let activeSec;
</script>

<div
  id="about"
  class="w-full h-screen z-30 bg-black/[.23] grid place-items-center"
>
  <div
    class="rounded-[12px] bg-white flex flex-col items-start justify-center w-[85%] p-6 relative"
  >
    <button
      class="absolute top-4 right-4 h-[20px] w-[20px]"
      on:click={() => (activeSec = null)}
    >
      <X />
    </button>
    <h4 class="text-[#042144] font-semibold mb-2">About View</h4>
    <span class="text-[#828282] font-medium text-sm leading-[16.8px]">
      Experience the power of AI-driven video analysis solutions in our
      cutting-edge Video Management Software. Explore the features of our
      advanced video management platform.
    </span>
    <button class="text-[#015a62] font-semibold text-base ml-auto mt-4">
      Learn more
    </button>
  </div>
</div>
