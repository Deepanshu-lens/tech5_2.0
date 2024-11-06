<script type="ts">
    import { selectedNode } from "@/stores";
  import { onMount } from "svelte";
  // import { addUserLog } from "@/lib/addUserLog";
  import * as Tabs from "@/components/ui/tabs";
  let activeTab = "live";
  let delayOptions = [
    { id: 1, label: "No delay" },
    { id: 2, label: "Shortest delay" },
    { id: 3, label: "Best quality" },
  ];
  let delaytype = 1;
  let resoOptions = [
    { id: 1, label: "240p" },
    { id: 2, label: "360p" },
    { id: 3, label: "480p" },
    { id: 4, label: "720p" },
    { id: 5, label: "1080p" },
    { id: 6, label: "default" },
  ];
  let resoType = 6;
  let dimensions = {};

  function handleConfirm() {
    console.log(resoType);
    console.log(delaytype);
  }

  onMount(() => {
    if ($selectedNode !== undefined) {
      delaytype = $selectedNode.live_mode === 0 ? 1 : $selectedNode.live_mode;
      resoType =
        $selectedNode.live_resolution === 0 ? 6 : $selectedNode.live_resolution;
    }
  });
</script>

<div
  class="flex flex-col items-start mt-12 h-[calc(100vh-75px)] w-full text-black dark:text-white"
>
  <Tabs.Root
    value={activeTab}
    onValueChange={(value) => (activeTab = value)}
    class="w-[94.5%] mx-6 shadow-none"
  >
    <Tabs.List
      class="h-[2.5rem] grid w-56 mx-auto border-neutral-200 grid-cols-2 shadow-none bg-white border"
    >
      <Tabs.Trigger
        value="live"
        class="data-[state=active]:bg-primary data-[state=active]:text-white"
        >Live</Tabs.Trigger
      >
      <Tabs.Trigger
        value="resolution"
        class="data-[state=active]:bg-primary data-[state=active]:text-white"
        >Resolution</Tabs.Trigger
      >
    </Tabs.List>
    <Tabs.Content value="live" class="mt-4">
      <div
        class="w-full pt-8 shad border-black/[.08] border-[1px] border-solid mb-12"
      >
        <h2 class="px-6 font-medium mb-4">Stream delay duration</h2>
        <div
          class="h-[1px] dark:bg-[#292929] ml-2 mb-4 bg-[#e0e0e0] mt-8 w-[95%]"
        />
        <form class="flex items-center justify-center w-[60%] mb-8 px-6">
          {#each delayOptions as { id, label }}
            <div class="flex gap-4 w-full">
              <label>
                <input
                  type="radio"
                  name="delayOption"
                  class="cursor-pointer checked:bg-[#0B8995] checked:border-[#0B8995]"
                  bind:group={delaytype}
                  value={id}
                />
                {label}
              </label>
            </div>
          {/each}
        </form>
        <p
          class={`px-6 mt-4 mb-8 text-sm opacity-90 animate-fade ${
            delaytype === 1
              ? "text-[#3B7C26] "
              : delaytype === 2
                ? "text-[#FB8A2E] "
                : "text-[#EF4444] "
          }`}
        >
          <span class="dark:text-[#929292] text-black/[.8] text-sm mr-1"
            >Note:</span
          >
          {delaytype === 1
            ? "Frame lag possible"
            : delaytype === 2
              ? "Frame consistent but quality issues can appear at times"
              : "Will run few seconds behind live but ensures consistent high quality stream"}
        </p>
      </div>
      <div
        class="w-full pt-8 shad border-black/[.08] border-[1px] border-solid"
      >
        <h2 class="px-6 font-medium mb-4">Live stream resolution</h2>
        <div
          class="h-[1px] dark:bg-[#292929] ml-2 mb-4 bg-[#e0e0e0] mt-8 w-[95%]"
        />
        <form class="flex items-center justify-center w-[70%] mb-8 px-6">
          {#each resoOptions as { id, label }}
            <div class="flex gap-4 w-full">
              <label>
                <input
                  type="radio"
                  class="cursor-pointer"
                  bind:group={resoType}
                  value={id}
                />
                {label}
              </label>
            </div>
          {/each}
        </form>
        <p
          id="fadeElement"
          class={`px-6 mt-4 mb-8 text-sm opacity-90 ${
            resoType === 1
              ? "text-[#3B7C26]"
              : resoType === 2
                ? "text-[#FB8A2E]"
                : resoType === 3
                  ? "text-[#FB8A2E]"
                  : resoType === 4
                    ? "text-[#EF4444]"
                    : resoType === 5
                      ? "text-[#EF4444]"
                      : "text-[#3B7C26]"
          }`}
        >
          <span class="dark:text-[#929292] text-black/[.8] text-sm mr-1"
            >Note:</span
          >
          {resoType === 1
            ? "For smooth lag, free streams"
            : resoType === 2
              ? "Frame consistent but quality issues"
              : resoType === 3
                ? "Minor lag could be there"
                : resoType === 4
                  ? "Intermittent frames lag could be observed"
                  : resoType === 5
                    ? "Frame lag is expected frequently"
                    : "Decided based on the number of cameras"}
        </p>
      </div>
    </Tabs.Content>
    <Tabs.Content value="resolution" class="mt-4">
      <div
        class="w-full pt-8 shad border-black/[.08] border-[1px] border-solid"
      >
        <h2 class="px-6 font-medium mb-4">Live stream resolution</h2>
        <div
          class="h-[1px] dark:bg-[#292929] ml-2 mb-4 bg-[#e0e0e0] mt-8 w-[95%]"
        />
        <form class="flex items-center justify-center w-[70%] mb-8 px-6">
          {#each resoOptions as { id, label }}
            <div class="flex gap-4 w-full">
              <label>
                <input
                  type="radio"
                  class="cursor-pointer"
                  bind:group={resoType}
                  value={id}
                />
                {label}
              </label>
            </div>
          {/each}
        </form>
        <p
          id="fadeElement"
          class={`px-6 mt-4 mb-8 text-sm opacity-90 ${
            resoType === 1
              ? "text-[#3B7C26]"
              : resoType === 2
                ? "text-[#FB8A2E]"
                : resoType === 3
                  ? "text-[#FB8A2E]"
                  : resoType === 4
                    ? "text-[#EF4444]"
                    : resoType === 5
                      ? "text-[#EF4444]"
                      : "text-[#3B7C26]"
          }`}
        >
          <span class="dark:text-[#929292] text-black/[.8] text-sm mr-1"
            >Note:</span
          >
          {resoType === 1
            ? "For smooth lag, free streams"
            : resoType === 2
              ? "Frame consistent but quality issues"
              : resoType === 3
                ? "Minor lag could be there"
                : resoType === 4
                  ? "Intermittent frames lag could be observed"
                  : resoType === 5
                    ? "Frame lag is expected frequently"
                    : "Decided based on the number of cameras"}
        </p>
      </div>
    </Tabs.Content>
  </Tabs.Root>

  <button
    class="text-white bg-[#015a62] px-10 py-2 rounded-lg mx-6 my-4"
    on:click={handleConfirm}
  >
    Confirm
  </button>
</div>

<style>
  .shad {
    box-shadow: 0px 4px 4px 0px #0000000f;
  }
</style>
