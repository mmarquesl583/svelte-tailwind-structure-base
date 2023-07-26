<script>
  import WebSocket from "./components/ws/WebSocket.svelte";
</script>

<div class="flex justify-around p-3 bg-slate-100">
  <div>
    <i class="text-4xl fa-solid fa-wifi"></i>
    <span class="uppercase text-2xl font-bold">
      INTERNET
    </span>
  </div>
  <div>
    <i class="text-4xl fa-solid fa-phone-volume"></i>
    <span class="uppercase text-2xl font-bold">
      VOZ
    </span>
  </div>
  <div>
    <i class="text-4xl fa-solid fa-camera"></i>
    <span class="uppercase text-2xl font-bold">
      IMAGEM
    </span>
  </div>
  <div>
    <i class="text-4xl fa-solid fa-keyboard"></i>
    <span class="uppercase text-2xl font-bold">
      JFL'S
    </span>
  </div>
  <div>
    <i class="text-4xl fa-solid fa-door-open"></i>
    <span class="uppercase text-2xl font-bold">
      C. ACESSO
    </span>
  </div>
</div>

<div class="grid grid-cols-3 gap-1 lg:grid-cols-5 p-1">
  <WebSocket />
</div>