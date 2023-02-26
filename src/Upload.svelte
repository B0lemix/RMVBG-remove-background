<script lang="ts">
  import { ImageStatus } from "../types.d";
  import Dropzone from "dropzone";
  import "dropzone/dist/dropzone.css";
  import { backgroundRemoval } from "@cloudinary/url-gen/actions/effect";
  import { Cloudinary } from "@cloudinary/url-gen";
  import { onMount } from "svelte";
  import { imageStatus, modifiedImage, originalImage } from "./store";

  const cloudinary = new Cloudinary({
    cloud: {
      cloudName: "dmjougutv",
    },

    url: {
      secure: true,
    },
  });

  onMount(() => {
    const dropzone = new Dropzone("#dropzone", {
      uploadMultiple: false,
      acceptedFiles: ".jpg,.png,.webp",
      maxFiles: 1,
    });

    dropzone.on("sending", (file, xhr, formData) => {
      imageStatus.set(ImageStatus.UPLOADING);
      formData.append("upload_preset", "vs0eiqxc");
      formData.append("timestamp", Date.now() / 1000);
      formData.append("api_key", "427478832387143");
    });

    dropzone.on("success", (file, response) => {
      const { public_id: publicId, secure_url: url } = response;

      //crear la imagen con fondo trasnparente y guardar en el backgroundImage
      const imageWithoutBackground = cloudinary
        .image(publicId)
        .effect(backgroundRemoval());

      modifiedImage.set(imageWithoutBackground.toURL());

      originalImage.set(url);
      imageStatus.set(ImageStatus.DONE);

      console.log(response);
    });

    /*     
        dropzone.on('error', (file,response)=>{
            console.log('COrrecto');
            console.log(response);
            
            
        })
 */

    dropzone.on("success", (file, response) => {
      console.log("COrrecto");
      console.log(response);
    });
  });
</script>

<form
  id="dropzone"
  action="https://api.cloudinary.com/v1_1/dmjougutv/image/upload"
  class="font bold shadow-2xl border-dashed border-2 border-gray-300 rounded-lg aspect-video w-full flex items-center justify-center flex-col"
>
  {#if $imageStatus === ImageStatus.READY}
    <button
      class="pointer-events-none bg-green-700 rounded-full text-bold px-6 py-4 text-white "
      >Upload</button
    >
    <strong class="text-lg mt-4 text-gray-600">or drop a file</strong>
  {/if}

  {#if $imageStatus === ImageStatus.UPLOADING}
    <strong class="text-lg mt-4 text-gray-600">Uploading files....</strong>
  {/if}
</form>
