<script>
  import Card from "../card/Card.svelte";
  const socket = new WebSocket("ws://localhost:8090");
  // const socket = new WebSocket("ws://192.168.3.28:8090");

  let device_list = []
  let payload = []
  let groupedDeviceList = []

  let conds = [
    {
      devices: [
        { origin: "Acácia",name: "Internet", ip: "10.0.27.1"},
        { origin: "Acácia",name: "VoIp", ip: "10.0.27.3"},
        { origin: "Acácia",name: "Cam1", ip: "10.0.27.11"},
        { origin: "Acácia",name: "JFL1", ip: "10.0.27.27"},
        { origin: "Acácia",name: "ControlId1", ip: "10.0.27.31"},
        { origin: "Acácia",name: "ControlId2", ip: "10.0.27.32"},
      ]
    },
    {
      devices: [
        { origin: "Albatroz",name: "Internet", ip: "10.0.56.1"},
        { origin: "Albatroz",name: "VoIp", ip: "10.0.56.5"},
        { origin: "Albatroz",name: "Cam1", ip: "10.0.56.11"},
        { origin: "Albatroz",name: "JFL1", ip: "10.0.56.25"},
        { origin: "Albatroz",name: "ControlId1", ip: "10.0.56.31"},
        { origin: "Albatroz",name: "ControlId2", ip: "10.0.56.32"},
      ]
    },
  ]

  let count = 0
  for(let cond of conds){
    for(let device of cond.devices){
      payload[count] = device
      count++
    }
  }

// conds = conds2;

    socket.onopen = (event) => {
      console.log("Conexão estabelecida.");
      setTimeout(() => {window.location.reload()}, 15000)
      // socket.send(JSON.stringify({interval: 30}))
      // socket.send(JSON.stringify({devices: payload}))
    };

    socket.onmessage = (event) => {
        const data = JSON.parse(event.data);
        if("event" in data){
          console.log(data.data.device)
          groupedDeviceList.forEach(cond => {
            cond.forEach(device => {
              if(device.ip == data.data.device.ip){
                device.online = data.data.device.online
              }
            });
          });
          // console.log(groupedDeviceList)
        }
        else if("devices" in  data){
          device_list = data.devices
          groupedDeviceList = groupDevicesByOrigin(device_list);
        }
    };

    function groupDevicesByOrigin(devices) {
      const groupedDevices = {};
      for (const device of devices) {
        if (!groupedDevices[device.origin]) {
          groupedDevices[device.origin] = [];
        }
        groupedDevices[device.origin].push(device);
      }
      return Object.values(groupedDevices);
    }
</script>

{#each groupedDeviceList as data}
  <div>
    <Card {data}/>
  </div>
{/each}