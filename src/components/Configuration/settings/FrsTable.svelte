<script>
    import * as Table from "@/components/ui/table/index";
</script>

<Table.Root    class="mx-auto h-full w-full flex flex-col max-w-[calc(100vw-8.3rem)] max-h-[calc(100vh-310px)] hide-scrollbar overflow-x-auto"
>
    <Table.Header
      class="border-2 border-[#e4e4e4] border-solid rounded-lg bg-[#f9f9f9]"
    >
      <Table.Row
        class="bg-transparent flex items-center justify-between p-3"
      >
        <Table.Head class="text-[#727272] h-full">User role</Table.Head>
        <Table.Head class="text-[#727272] h-full">Feature 1</Table.Head>
        <Table.Head class="text-[#727272] h-full">Feature 2</Table.Head>
        <Table.Head class="text-[#727272] h-full">Feature 3</Table.Head>
        <Table.Head class="text-[#727272] h-full">Feature 4</Table.Head>
      </Table.Row>
    </Table.Header>
    <Table.Body
      class="overflow-y-scroll max-h-[calc(100vh-285px)] hide-scrollbar pb-10"
    >
      <Table.Row
        class="bg-transparent cursor-pointer flex items-center justify-between gap-4 mt-4 px-3 rounded-lg  border-2 border-solid border-[#e4e4e4]"
      >
        <Table.Cell class="text-black h-full"
          ><span
            class="flex items-center gap-2 capitalize font-semibold"
          >
            user 1
          </span>
        </Table.Cell>
        <Table.Cell class="text-[#727272] h-full text-sm ">
          <input type="checkbox" />
        </Table.Cell>
        <Table.Cell class="text-[#727272] h-full text-sm">
          <input type="checkbox" />
        </Table.Cell>
        <Table.Cell class="text-[#727272] h-full text-sm ">
          <input type="checkbox" />
        </Table.Cell>
        <Table.Cell class="text-[#727272] h-full  ">
          <input type="checkbox" />
        </Table.Cell>
      </Table.Row>
    </Table.Body>
  </Table.Root>