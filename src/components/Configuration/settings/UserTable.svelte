<script lang="ts">
  import { Button } from "@/components/ui/button";
  import { Input } from "@/components/ui/input";
  import {
    Table,
    TableBody,
    TableCell,
    TableHead,
    TableHeader,
    TableRow,
  } from "@/components/ui/table";
  import { Checkbox } from "@/components/ui/checkbox";
  import {
    SlidersHorizontal,
    ChevronDown,
    Plus,
    ArrowDown,
    SquarePen,
  } from "lucide-svelte";

  // Mock data for the table
  const users = [
    {
      name: "Sam Singh",
      email: "sam.singh@gmail.com",
      lastActivity: "Thu, March 21, 2024",
      logEvent: "Changed Password from users",
    },
    {
      name: "Rahul Singh",
      email: "rahul.singh@gmail.com",
      lastActivity: "Thu, March 21, 2024",
      logEvent: "Added new face to FRS",
    },
    {
      name: "Rahul Singh",
      email: "rahul.singh@gmail.com",
      lastActivity: "Thu, March 21, 2024",
      logEvent: "Added new face to FRS",
    },
    {
      name: "Rahul Singh",
      email: "rahul.singh@gmail.com",
      lastActivity: "Thu, March 21, 2024",
      logEvent: "Added new face to FRS",
    },
    {
      name: "Rahul Singh",
      email: "rahul.singh@gmail.com",
      lastActivity: "Thu, March 21, 2024",
      logEvent: "Added new face to FRS",
    },
    {
      name: "Rahul Singh",
      email: "rahul.singh@gmail.com",
      lastActivity: "Thu, March 21, 2024",
      logEvent: "Added new face to FRS",
    },
  ];
</script>

<div class="p-6 bg-white rounded-lg w-full">
  <div class="border border-gray-200 rounded-lg shadow-sm">
    <!-- Table Heading -->
    <div class="flex justify-between items-center mb-4 px-5 pt-4">
      <div>
        <div class="flex items-center gap-2 my-1">
          <h2 class="text-xl font-semibold">Users</h2>
          <span
            class="text-sm text-black/.5 bg-[#F7FAFF] rounded-lg px-2 py-1 text-[#0070FF]"
            >Super Admin</span
          >
        </div>
        <p class="text-sm text-gray-500">Record of user & log events</p>
      </div>
      <div class="flex items-center space-x-2">
        <Button variant="outline" class="flex items-center">
          <SlidersHorizontal class="w-4 h-4 mr-2" />
          Filters
        </Button>
        <Button variant="outline" class="flex items-center">
          Node
          <ChevronDown class="w-4 h-4 ml-2" />
        </Button>
        <Button variant="outline" class="flex items-center">
          Role
          <ChevronDown class="w-4 h-4 ml-2" />
        </Button>
        <Button variant="brand">
          <Plus class="w-4 h-4 mr-2" />
          Add User
        </Button>
      </div>
    </div>

    <Table>
      <TableHeader class="bg-[#EAECF0]/50">
        <TableRow>
          <TableHead class="w-12">
            <Checkbox />
          </TableHead>
          <TableHead
            ><span class="flex items-center gap-1"
              >User Name
              <ArrowDown size={15} /></span
            >
          </TableHead>
          <TableHead
            ><span class="flex items-center gap-1"
              >Email Address
              <ArrowDown size={15} /></span
            ></TableHead
          >
          <TableHead>
            <span class="flex items-center gap-1"
              >Last Activity
              <ArrowDown size={15} /></span
            >
          </TableHead>
          <TableHead>
            <span class="flex items-center gap-1"
              >Log Event
              <ArrowDown size={15} /></span
            >
          </TableHead>
          <TableHead>Action</TableHead>
        </TableRow>
      </TableHeader>
      <TableBody>
        {#each users as user}
          <TableRow>
            <TableCell>
              <Checkbox />
            </TableCell>
            <TableCell>{user.name}</TableCell>
            <TableCell class="text-neutral-500">{user.email}</TableCell>
            <TableCell class="text-neutral-500">{user.lastActivity}</TableCell>
            <TableCell class="text-neutral-500">{user.logEvent}</TableCell>
            <TableCell>
              <Button
                size="xs"
                variant="ghost"
                class="bg-[#F16522]/10 rounded-3xl text-[#F16522] py-3 px-3"
              >
                <SquarePen class="w-4 h-4 mr-1" />
                Edit
              </Button>
            </TableCell>
          </TableRow>
        {/each}
      </TableBody>
    </Table>
  </div>
</div>
