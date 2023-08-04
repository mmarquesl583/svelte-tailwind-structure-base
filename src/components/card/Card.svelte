<script>
  import { afterUpdate } from "svelte";

  export let data;

  let color;
  let ips_off = [];

  let types = {
    internet: {
      class:"text-4xl fa-solid fa-wifi",
      online: true,
      counter: 0
    },
    imagem: {
      class:"text-4xl fa-solid fa-camera",
      online: true,
      counter: 0
    },
    voz: {
      class:"text-4xl fa-solid fa-phone-volume",
      online: true,
      counter: 0
    },
    jfl: {
      class:"text-4xl fa-solid fa-keyboard",
      online: true,
      counter: 0
    },
    acesso: {
      class:"text-4xl fa-solid fa-door-open",
      online: true,
      counter: 0
    },
  }

  function getType(type){
    const get = types[type]
    get.counter++
    return get
  }

  function main() {
    let trigger = false
    let type

    for(let i = 0; i < data.devices.length; i++){
      if(data.devices[i].online == false){
        type = getType(data.devices[i].type)
        trigger = true
      }
    }
    color = trigger? "red" :"green"

    for (let t in types) {
      var found = false
      var internet = false
      for(let i = 0; i < data.devices.length; i++) {
        if(data.devices[i].type == "internet" && !data.devices[i].online){
          internet = true
          types["internet"].online = false
          types["internet"].counter++
          ips_off.push("oi")
          break
        }
        else if (data.devices[i].type == t && !data.devices[i].online) {
          types[data.devices[i].type].online = false
          types[data.devices[i].type].counter++
          found = true
          break
        }
      }
      if (internet) break
      if (!found) types[t].online = true
    }
    // console.log(ips_off)
  }

  afterUpdate(()=>{
    main()
  })
</script>

<div class="card w-auto border border-black p-3 relative bg-{color}-500 opacity-70 ">
  <div class="name uppercase font-bold text-2xl border-b-2 items-start ml-2">
    <i class="fa-solid fa-building mr-1"></i>
    {data.name}
  </div>
  <div class="items-end w-full h-auto">
    <div class="text-lg h-auto font-bold ml-2 mt-2 flex ">
      {#if ips_off.length === 1}
        <span>{ips_off[0]}</span>
      {:else}
        {#each ips_off as ip_off}
          {#if ip_off != ips_off[0]}
            <span> - </span>
          {/if}
          <span>{ip_off}</span>
        {/each}
      {/if}
    </div>
    <div class="flex w-full justify-around">
      {#each Object.entries(types) as [name, content]}
        <div class="relative alpha-{content.online ? "1" : "2"}">
          <i class={content.class}></i>
        </div>
        <!-- <div class="relative">
          {#if content.counter > 0}
            <span class="bg-yellow-300 bottom-0 -ml-6 rounded-full absolute font-bold font-mono text-xs px-1">
              {content.counter}
            </span>
          {/if}
        </div> -->
      {/each}
    </div>
  </div>
</div>
