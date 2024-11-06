<script lang="ts">
  import {
    ChevronLeft,
    Clock10Icon,
    Mic,
    SendHorizonalIcon,
  } from "lucide-svelte";

  export let activeSec;
</script>

<div
  id="help Center"
  class="w-full h-screen z-30 bg-black/[.23] grid place-items-center"
>
  <div
    class="rounded-[12px] bg-[#F5F6F7] flex flex-col items-start w-screen h-screen relative"
  >
    <button
      class="flex items-center justify-start text-black/[.7] pt-4"
      on:click={() => (activeSec = null)}
    >
      <ChevronLeft class="h-[30px] w-[30px]" />
      <p class="text-lg font-semibold">Help Centre</p>
    </button>
    <div class="flex flex-col gap-6">
      <img
        src="/images/helpcenter.png"
        alt="vector"
        class="flex-shrink-0 w-[290px] h-[290px] mx-auto aspect-square object-contain"
      />
      <h2 class="text-2xl font-semibold text-center text-black px-6">
        How can we help you today?
      </h2>
      <p class="text-[#505050] font-semibold text-base text-center px-4">
        Start asking questions or type your queries here, our Tru AI will answer
        it all.
      </p>
      <span
        class="relative bg-[#015a62] text-white/[.70] mx-4 rounded-lg h-[42px] w-[360px] text-sm"
      >
        <Mic
          class="absolute left-2 top-1/2 -translate-y-1/2 h-[18px] w-[18px]"
        />
        <input
          class=" bg-transparent px-10 placeholder-white/[.70] h-[42px] w-[360px]"
          placeholder="type or speak anything!"
        />
        <span
          class="flex absolute flex-row items-center justify-center gap-2 right-2 top-1/2 -translate-y-1/2"
        >
          <SendHorizonalIcon class="h-[16px] w-[16px]" />
          <span class="w-[1px] h-[20px] bg-white/[.70]" />
          <Clock10Icon class="h-[16px] w-[16px]" />
        </span>
      </span>
    </div>
  </div>
</div>
