<script>
  import { ChevronDown, ChevronLeft } from "lucide-svelte";
  import {
    Disc2,
    Router,
    Radio,
    Cast,
    CameraIcon,
    User2Icon,
  } from "lucide-svelte";
  import { writable } from "svelte/store";
  import Remote from "./settings/Remote.svelte";
  import Camera from "./settings/Camera.svelte";
  import Recording from "./settings/Recording.svelte";
  import Stream from "./settings/Stream.svelte";
  import System from "./settings/System.svelte";
  import User from "./settings/User.svelte";

  export let activeSec;

  const buttonData = [
    { text: "Remote", icon: "mingcute:remote-line" },
    { text: "Stream", icon: "solar:play-stream-linear" },
    { text: "Recording", icon: "fontisto:radio-btn-active" },
    { text: "System", icon: "ph:screencast-bold" },
    { text: "Camera", icon: "lucide:camera" },
    { text: "User", icon: "ci:user-01" },
  ];

  let selected = writable(null);
  export let userData;
  export let user;
  export let records;
</script>

<div
  id="Configuration"
  class="w-full min-h-screen z-30 bg-black/[.23] grid place-items-center overflow-y-auto h-[calc(100vh-200px)] pb-32"
>
  <div
    class="rounded-[12px] bg-[#F5F6F7] flex flex-col items-start w-screen min-h-screen relative"
  >
    <button
      class="flex items-center justify-start text-black/[.7] pt-4"
      on:click={() => (activeSec = null)}
    >
      <ChevronLeft class="h-[30px] w-[30px]" />
      <p class="text-lg font-semibold text-black">Configuration</p>
    </button>
    <div
      class="flex flex-col items-center justify-center w-full my-6 gap-[30px] mt-[40px]"
    >
      {#each buttonData as { text }}
        {#if $selected === "Remote" && text === "Remote"}
          <Remote {text} bind:selected={$selected} />
        {:else if $selected === "Stream" && text === "Stream"}
          <Stream {text} bind:selected={$selected} />
        {:else if $selected === "Recording" && text === "Recording"}
          <Recording {text} bind:selected={$selected} />
        {:else if $selected === "System" && text === "System"}
          <System {text} bind:selected={$selected} />
        {:else if $selected === "Camera" && text === "Camera"}
          <Camera {text} bind:selected={$selected} {user} />
        {:else if $selected === "User" && text === "User"}
          <User {text} bind:selected={$selected} {userData} {records} />
        {:else}
          <button
            on:click={() => ($selected = text)}
            class="flex flex-row items-center w-[348px] h-[50px] px-[19px] py-[13px] border-b-[1px] border-solid border-[#e0e0e0] rounded-[8px]"
            style="margin-bottom: {text === 'User' ? '5rem' : '0rem'}"
          >
            <!-- <SvgComponent name={icon} height={30} width={30} color="#919eab" class="mr-4" viewBox={text === 'System' ? '0 0 256 256' : '0 0 24 24'} /> -->
            {#if text === "Remote"}
              <Router class="text-[#919eab] mr-4" />
            {:else if text === "Stream"}
              <Radio class="text-[#919eab] mr-4" />
            {:else if text === "Recording"}
              <Disc2 class="text-[#919eab] mr-4" />
            {:else if text === "System"}
              <Cast class="text-[#919eab] mr-4" />
            {:else if text === "Camera"}
              <CameraIcon class="text-[#919eab] mr-4" />
            {:else}
              <User2Icon class="text-[#919eab] mr-4" />
            {/if}
            <span class="text-[#919eab]">{text}</span>
            <ChevronDown class=" ml-auto h-[30px] w-[30px]" />
          </button>
        {/if}
      {/each}
    </div>
  </div>
</div>
