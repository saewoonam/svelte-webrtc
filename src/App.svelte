<script>
  import {onMount} from 'svelte';
  import Quagga from '@ericblade/quagga2'; // ES6
  console.log('Quagga', Quagga);
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
