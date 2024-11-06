<script lang="ts">
    import { Switch } from "@/components/ui/switch";
    import { selectedNode } from "@/stores";
    import { ChevronLeft, PlusCircle } from "lucide-svelte";
  
    let username: string = "";
    let name: string = "";
    let ip: string = "";
    let company: string = "";
    let disabled = "";
    let url = "";
    let pass = "";
    let saving: boolean = false;
    let vehicle: boolean = false;
    let face: boolean = false;
    export let activeSec;
    console.log(activeSec);
  </script>
  
  <div
    class="rounded-[12px] bg-[#F5F6F7] flex flex-col items-start w-screen min-h-screen relative pb-32"
  >
    <button
      class="flex items-center justify-start text-black/[.7] pt-4"
      on:click={() => {
        activeSec = null;
      }}
    >
      <ChevronLeft class="text-black h-[30px] w-[30px]" />
      <p class="text-lg font-semibold text-black">Add Device</p>
    </button>
    <div
      class="flex flex-col mt-[30px] gap-6 mx-auto overflow-y-scroll overflow-x-clip max-h-[calc(100vh-200px)] pb-32 hide-scrollbar"
    >
      <p class="text-black">
        Insert a new camera to node : <span class="font-bold">
          {$selectedNode && $selectedNode.name}
        </span>
      </p>
      <span
        class="relative h-[48px] w-[335px] rounded-[8px] border-[1px] border-solid border-[rgb(145,158,171)]/[.32]"
      >
        <span
          class="absolute -top-3 left-6 text-sm font-medium text-[#637381] bg-[#f5f6f7]"
        >
          Camera Name
        </span>
        <input
          class="h-full w-full leading-[16.8px] text-sm text-[#919eab] placeholder-[#919eab] bg-transparent px-6"
          autoComplete="off"
          autoCapitalize="off"
          placeholder="Enter camera name"
          type="text"
          on:change={(e) => (name = e.target.value)}
        />
      </span>
      <p class="text-black text-xl font-semibold text-center">Camera Details</p>
      <span
        class="relative h-[48px] w-[335px] rounded-[8px] border-[1px] border-solid border-[rgb(145,158,171)]/[.32]"
      >
        <span
          class="absolute -top-3 left-6 text-sm font-medium text-[#637381] bg-[#f5f6f7]"
        >
          Username
        </span>
        <input
          class="h-full w-full leading-[16.8px] text-sm text-[#919eab] placeholder-[#919eab] bg-transparent px-6"
          autoComplete="off"
          placeholder="Enter username"
          type="text"
          autoCapitalize="off"
          on:change={(e) => (username = e.target.value)}
          disabled={disabled === "other"}
        />
      </span>
      <span
        class="relative h-[48px] w-[335px] rounded-[8px] border-[1px] border-solid border-[rgb(145,158,171)]/[.32]"
      >
        <span
          class="absolute -top-3 left-6 text-sm font-medium text-[#637381] bg-[#f5f6f7]"
        >
          Password
        </span>
        <input
          class="h-full w-full leading-[16.8px] text-sm text-[#919eab] placeholder-[#919eab] bg-transparent px-6"
          autoComplete="off"
          placeholder="Enter password"
          autoCapitalize="off"
          type="Password"
          on:change={(e) => (pass = e.target.value)}
          disabled={disabled === "other"}
        />
      </span>
      <span
        class="relative h-[48px] w-[335px] rounded-[8px] border-[1px] border-solid border-[rgb(145,158,171)]/[.32]"
      >
        <span
          class="absolute -top-3 left-6 text-sm font-medium text-[#637381] bg-[#f5f6f7]"
        >
          IP address
        </span>
        <input
          class="h-full w-full leading-[16.8px] text-sm text-[#919eab] placeholder-[#919eab] bg-transparent px-6"
          autoComplete="off"
          autoCapitalize="off"
          placeholder="Enter IP address"
          type="text"
          value={ip}
          on:change={(e) => (ip = e.target.value)}
          disabled={disabled === "other"}
        />
      </span>
      <span class="flex items-center justify-center mx-auto text-black gap-4">
        <input
          type="checkbox"
          on:change={() => {
            if (company !== "1") {
              company = "1";
            } else {
              company = "";
            }
          }}
          disabled={disabled === "other"}
          class="disabled:cursor-not-allowed flex-shrink-0"
          checked={company === "1"}
        />
        <span>CP Plus</span>
        <input
          type="checkbox"
          on:change={() => {
            if (company !== "2") {
              company = "2";
            } else {
              company = "";
            }
          }}
          class="disabled:cursor-not-allowed flex-shrink-0"
          disabled={disabled === "other"}
          checked={company === "2"}
        />
        <span>HikVision</span>
        <input type="checkbox" />
        <span>Other</span>
      </span>
      <p class="text-black text-xl font-semibold text-center my-3">
        Or you can add URL path
      </p>
      <span
        class="relative h-[48px] w-[335px] rounded-[8px] border-[1px] border-solid border-[rgb(145,158,171)]/[.32]"
      >
        <span
          class="absolute -top-3 left-6 text-sm font-medium text-[#637381] bg-[#f5f6f7]"
        >
          Full URL path
        </span>
        <input
          class="h-full w-full leading-[16.8px] text-sm text-[#919eab] placeholder-[#919eab] bg-transparent px-6"
          autoComplete="off"
          autoCapitalize="off"
          placeholder="proptocol://username:password@ip-address"
          type="text"
          onChange={(e) => (url = e.target.value)}
          disabled={disabled === "url"}
        />
      </span>
      <p class="text-black text-xl font-semibold text-center my-3">
        Camera Features
      </p>
      <div class="flex items-center justify-evenly w-full">
        <!-- svelte-ignore a11y-label-has-associated-control -->
        <label
          class=" text-sm font-medium leading-6 dark:text-white text-[#2c2c2c] flex items-center gap-1"
        >
          <Switch bind:checked={saving} class="scale-90" />
          Feed Saving
        </label>
        <!-- svelte-ignore a11y-label-has-associated-control -->
        <label
          class=" text-sm font-medium leading-6 dark:text-white text-[#2c2c2c] flex items-center gap-1"
        >
          <Switch bind:checked={vehicle} class="scale-90" />
          Vehicle Scan
        </label>
      </div>
      <div class="flex items-center justify-center w-full">
        <!-- svelte-ignore a11y-label-has-associated-control -->
        <label
          class=" text-sm font-medium leading-6 dark:text-white text-[#2c2c2c] flex items-center gap-1"
        >
          <Switch bind:checked={face} class="scale-90" />
          Face Scan
        </label>
      </div>
      <button
        class="flex gap-2 px-[120px] py-[15px] bg-[#015a62] text-white mx-auto mt-6 rounded-[8px] justify-center items-center"
      >
        <PlusCircle class="h-[20px] w-[20px]" />
        Add Node
      </button>
    </div>
  </div>
  