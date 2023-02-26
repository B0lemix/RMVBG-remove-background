<script lang="ts">
  import { ImageStatus } from "../types.d";
  import "./app.css";
  import Edit from "./Edit.svelte";
  import { imageStatus } from "./store";
  import Upload from "./Upload.svelte";
</script>

<div
  class="max-w-xl m-auto grid grid-cols-1 place-content-center w-full h-screen p-4"
>
  <header class="flex justify-center py-10">
    <h1 class="text-6xl font-bold">
      <span class="text-green-800 tracking-tighter"
        >RMV<span class="text-green-600 tracking-tighter">BG</span></span
      ><span class="text-2xl text-gray-600 flex flex-nowrap"
        >&nbsp//remove background with AI</span
      >
    </h1>
  </header>

  <main class="w-full block">
    {#if $imageStatus === ImageStatus.READY || $imageStatus === ImageStatus.UPLOADING}
      <Upload />
    {:else if $imageStatus === ImageStatus.DONE}
      <Edit />
    {/if}
  </main>

  <footer class="flex justify-center items-center gap-x-2 font-semibold pt-10">
    Made using Svelte, Tailwind CSS, Cloudinary , Two-Up-Element & Dropzone JS
  </footer>
</div>
