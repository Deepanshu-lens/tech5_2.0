<script lang="ts">
    import PocketBase from "pocketbase";
    // import { page } from "$app/stores";
      import { getContext } from "svelte";
  
    export let session: any;
    let licenseOptions = [
      { id: 1, label: "FRS" },
      { id: 2, label: "ANPR" },
      { id: 3, label: "Playback" },
      { id: 4, label: "MARS" },
    ];
  
    let frs = session?.frs;
    let anpr = session?.anpr;
    let playback = session?.playback;
    let mars = session?.mars;
  
    const PB: PocketBase = getContext("pb");
  
    async function handleInputChange(event, label) {
      console.log(label, event.target.checked);
      // if (label === "FRS" && event.target.checked === false) {
      //   console.log("frs flase");
      //   const data = session;
      //   data.frs = false;
      //   data.playback = false;
      //   const record = await PB.collection("session").update(session?.id, data);
      //   console.log("pages updated", record);
      // } else {
      const changefield = label.toLowerCase();
      const data = session;
      data[changefield] = event.target.checked;
      const record = await PB.collection("session").update(session?.id, data);
      console.log("pages updated", record);
      // }
    }
  </script>
  
  <div
    class="flex flex-col items-start mt-12 h-[calc(100vh-75px)] w-full text-black dark:text-white"
  >
    <div
      class=" w-[94.5%] my-4 pt-8 shad mx-6 border-black/[.08] border-[1px] border-solid flex flex-col"
    >
      <div
        class="flex w-full justify-start items-center gap-20 font-bold mb-4 px-8"
      >
        <div class=" relative">
          <span class="font-bold text-[#015A62] dark:text-white"> License </span>
          <span
            class=" h-[3px] rounded-full bg-[#0B8995] w-full absolute left-0 -bottom-4"
          />
        </div>
      </div>
      <div class="h-[1px] w-[96%] dark:bg-[#242424] mb-8 bg-[#e0e0e0]" />
  
      <form class=" flex items-center justify-center gap-4 w-[75%] mb-8 px-8">
        <div class="flex gap-2 w-full">
          <input
            type="checkbox"
            name="frs"
            class="cursor-pointer"
            checked={frs}
            on:change={(e) => handleInputChange(e, licenseOptions[0].label)}
          />
          <!-- svelte-ignore a11y-label-has-associated-control -->
          <label>
            {licenseOptions[0].label}
          </label>
        </div>
        <div class="flex gap-2 w-full">
          <input
            type="checkbox"
            name="anpr"
            class="cursor-pointer"
            checked={anpr}
            on:change={(e) => handleInputChange(e, licenseOptions[1].label)}
          />
          <!-- svelte-ignore a11y-label-has-associated-control -->
          <label>
            {licenseOptions[1].label}
          </label>
        </div>
        <div class="flex gap-2 w-full">
          <input
            type="checkbox"
            name="playback"
            class="cursor-pointer"
            checked={playback}
            on:change={(e) => handleInputChange(e, licenseOptions[2].label)}
          />
          <!-- svelte-ignore a11y-label-has-associated-control -->
          <label>
            {licenseOptions[2].label}
          </label>
        </div>
        <div class="flex gap-2 w-full">
          <input
            type="checkbox"
            name="mars"
            class="cursor-pointer"
            checked={mars}
            on:change={(e) => handleInputChange(e, licenseOptions[3].label)}
          />
          <!-- svelte-ignore a11y-label-has-associated-control -->
          <label>
            {licenseOptions[3].label}
          </label>
        </div>
      </form>
    </div>
  </div>
  