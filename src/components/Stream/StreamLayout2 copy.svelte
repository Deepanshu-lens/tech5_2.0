<script lang="ts">
  import { onMount, afterUpdate } from "svelte";
  import AddNode from "@/components/Nodes/AddNode.svelte";
  import StreamTile from "./StreamTile.svelte";
  import { cameras, selectedLayout } from "@/stores";

  // let currentCameras = [];

  export let currentCameras = [];

  // $: currentCameras =
  //   false && $cameras.length > 0 && layout > 0 && layout <= 6
  //     ? $cameras.slice(0, layout * layout)
  //     : $cameras;

  // $: currentCameras = [...$cameras, ...Array(6).fill("")];
  // $: currentCameras = $cameras.splice(0, 11);

  export let layout = 0;

  let container;
  let gridItems = [];
  let columns = 1,
    rows = 1;
  $: ({ columns, rows } = calculateLayout(currentCameras.length));

  function calculateLayout(count) {
    if (count <= 2) return { columns: count, rows: 1 };
    if (count <= 4) return { columns: 2, rows: 2 };
    if (count <= 6) return { columns: 3, rows: 2 };
    if (count <= 9) return { columns: 3, rows: 3 };
    if (count <= 12) return { columns: 3, rows: 4 };
    if (count <= 16) return { columns: 4, rows: 4 };
    if (count <= 20) return { columns: 4, rows: 5 };
    if (count <= 25) return { columns: 5, rows: 5 };
    if (count <= 30) return { columns: 5, rows: 6 };
    if (count <= 36) return { columns: 6, rows: 6 };
    return { columns: 3, rows: Math.ceil(count / 3) };
  }

  function updateLayout() {
    if (!container || currentCameras.length === 0) return;
    if (layout > 0 && layout <= 6) {
      columns = rows = layout;
    }

    const containerWidth = container.clientWidth;
    const containerHeight = container.clientHeight;

    console.log(containerWidth);

    const aspectRatio = 16 / 9;
    let gap = 8; // Gap between items

    let itemWidth = (containerWidth - (columns + 1) * gap) / columns;
    let itemHeight = itemWidth / aspectRatio;

    // gap =
    //   gap +
    //   (containerWidth - (columns * itemWidth + gap * (columns - 1))) /
    //     (columns - 1);

    console.log(
      "old item width: ",
      itemWidth,
      columns * itemWidth + gap * (columns - 1),
      containerWidth,
    );
    itemWidth = (containerWidth - (columns + 1) * gap) / columns;
    console.log("new item width: ", itemWidth);
    itemHeight = itemWidth / aspectRatio;

    // Adjust height if it exceeds container height
    const totalHeight = rows * itemHeight + (rows + 1) * gap;
    if (totalHeight > containerHeight) {
      itemHeight = (containerHeight - (rows + 1) * gap) / rows;
      itemWidth = itemHeight * aspectRatio;
    }

    let vgap =
      (gap +
        (containerWidth - (columns * itemWidth + gap * (columns - 1))) /
          (columns - 1)) /
      2;

    const lastRowColumns = currentCameras.length % columns || columns;
    const lastRowWidth =
      (containerWidth - (lastRowColumns + 1) * vgap) / lastRowColumns;
    const lastRowHeight = lastRowWidth / aspectRatio;

    $: console.log(
      "NumCams: ",
      currentCameras.length,
      "\nContainer Width: ",
      containerWidth,
      "\nContainer Height: ",
      containerHeight,
      "\nTotal Height: ",
      totalHeight,
      "\nItem Width: ",
      itemWidth,
      "\nItemHeight: ",
      itemHeight,
      "\nRows: ",
      rows,
      "\nColumns: ",
      columns,
      "\nlastRowWidth: ",
      lastRowWidth,
      "\nlastRowColumns: ",
      lastRowColumns,
      "\nlastRowHeight: ",
      lastRowHeight,
      "\nleftoverWidth: ",
      containerWidth -
        (columns * itemWidth + gap * (columns - 1)) / (columns - 1),
    );

    gridItems.forEach((item, index) => {
      let width, height, top, left;
      const row = Math.floor(index / columns);
      const col = index % columns;

      if (row < rows - 1 || currentCameras.length % columns === 0) {
        // Regular grid items
        width = itemWidth;
        height = itemHeight;
        top = row * (itemHeight + gap) + gap;
        left = col * (itemWidth + vgap) + vgap;
      } else {
        // Last row items for odd number of currentCameras
        width = lastRowWidth;
        height = lastRowHeight;
        top = (rows - 1) * (itemHeight + gap) + gap;
        left = (col % lastRowColumns) * (lastRowWidth + vgap) + vgap;
      }

      item.style.width = `${width}px`;
      item.style.height = `${height}px`;
      item.style.top = `${top}px`;
      item.style.left = `${left}px`;
    });
  }

  function updateGridItems() {
    if (container) {
      gridItems = Array.from(container.querySelectorAll(".grid-item"));
      updateLayout();
    }
  }

  onMount(() => {
    updateGridItems();
    window.addEventListener("resize", updateLayout);
    return () => window.removeEventListener("resize", updateLayout);
  });

  afterUpdate(() => {
    updateGridItems();
  });
  $: {
    currentCameras;
    if (container) {
      setTimeout(updateGridItems, 0);
    }
  }
</script>

{#if currentCameras.length > 0}
  <div class="layout-container" bind:this={container}>
    {#each currentCameras as stream, i}
      <div class="grid-item rounded-lg">
        <StreamTile
          url={`
wss://view.lenscorp.cloud/api/ws?src=${stream.id}_FULL`}
          classes=""
        />
      </div>
    {/each}
  </div>
{/if}

<style>
  .layout-container {
    position: relative;
    /* width: 75vw; */
    height: calc(100vh - 7rem);
    overflow: hidden;
  }

  .grid-item {
    position: absolute;
    /* background-color: #f0f0f0; */
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 24px;
    color: #333;
  }
</style>
