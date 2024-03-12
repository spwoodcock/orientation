<script type="module" lang="ts">
  import hotLogo from './assets/hot_logo.png';
  import { Capacitor, PluginListenerHandle } from '@capacitor/core';
  import { Motion } from '@capacitor/motion';

  let accelHandler: PluginListenerHandle;

  const stopLocationTracking = async () => {
    console.log('STOP TRACKING')
    if (accelHandler) {
      accelHandler.remove();
    }
    Motion.removeAllListeners();
  };

  const startLocationTracking = async () => {
    console.log('START TRACKING')
    try {
      await DeviceMotionEvent.requestPermission();
    } catch (e) {
      console.log(e)
      return;
    }
  
    accelHandler = await Motion.addListener('accel', event => {
      console.log('Device motion event:', event);
    });

    accelHandler = await Motion.addListener('orientation', event => {
      console.log('Device orientation event:', event);
    });
  };

</script>

<main>
  <div>
    <a href="https://hotosm.org" target="_blank" rel="noreferrer">
      <img src={hotLogo} class="logo" alt="HOT Logo" />
    </a>
  </div>

  <h3>Start Tracking</h3>
  <button on:click={startLocationTracking}>Get Location</button>
  <h3>Stop Tracking</h3>
  <button on:click={stopLocationTracking}>Get Location</button>
  <br><br>
</main>

<style>
  .logo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
    transition: filter 300ms;
  }
  .logo:hover {
    filter: drop-shadow(0 0 2em #646cffaa);
  }
</style>
