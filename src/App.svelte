<script>
  import {onMount} from 'svelte';
  import Quagga from '@ericblade/quagga2'; // ES6
  let results;

  console.log('Quagga', Quagga);
  function detected(data) {
    console.log(data);
    results = JSON.stringify(data);
  }
  onMount( ()=>{
    let video = document.getElementById('video');
    navigator.mediaDevices.getUserMedia({
      video: { width: 400, height: 300 },
      audio: false
    })  
    .then(function(stream) {
      // console.log('stream', stream);
      video.srcObject = stream;
      video.play();
    })
    .catch(function(err) {
      console.log("An error occurred: " + err);
    });
    Quagga.init({
      inputStream : {
        name : "Live",
        type : "LiveStream",
        target: document.querySelector('#video')    // Or '#yourElement' (optional)
      },
      decoder : {
        readers : ["code_128_reader"]
      }
    }, function(err) {
      if (err) {
        console.log(err);
        return
      }
      console.log("Initialization finished. Ready to start");
      Quagga.onDetected(detected);
      Quagga.start();
    });
  });
  
  
</script>
<style>
.camera {
  width: 340px;
  display:inline-block;
}
</style>
  <div class="camera">
    <video id="video">Video stream not available.
      <track kind="captions" />
    </video>
  </div>
{#if results}
results: {results}
{/if}
