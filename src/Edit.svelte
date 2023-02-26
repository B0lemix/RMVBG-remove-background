<script lang="ts">
  import { originalImage, modifiedImage } from "./store";
  import "two-up-element";

  let processingImage = true;
  let tries = 0;
  let intervalId;

  //parecido a use effect

  $: {
    if (processingImage) {
      clearInterval(intervalId);
      intervalId = setInterval(() => {
        tries++;
        /*         if (tries>10){
            processingImage=false
        } */
        const img = new Image();
        img.src = $modifiedImage;
        img.onload = () => {
          processingImage = false;
          clearInterval(intervalId);
        };
      }, 500);
    }
  }
</script>

<two-up>
  <img src={$originalImage} alt="Imagen original subida por el user" />

  {#if processingImage}
    <div class="flex flex-col justify-center items-center">
      <div class="lds-ripple">
        <div />
        <div />
      </div>
      <p class="text-center font-bold mt-4">Procesando imagen...</p>
    </div>
  {:else}
    <img src={$modifiedImage} alt="Imagen modificada" />
  {/if}
</two-up>

<a
  download
  href={$modifiedImage}
  class="block bg-blue-500 text-xl w-full font-bold text-white rounded-full text-center px-4 py-2 mt-10 hover:bg-blue-700"
>
  Descargar imagen sin fondo
</a>
