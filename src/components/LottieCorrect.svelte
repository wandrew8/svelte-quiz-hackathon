<script>
  import { LottiePlayer } from "@lottiefiles/svelte-lottie-player";

  let controlsLayout = [
    "previousFrame",
    "playpause",
    "stop",
    "nextFrame",
    "progress",
    "frame",
    "loop",
    "spacer",
    "background",
    "snapshot",
    "zoom",
    "info",
  ];
</script>

<style>
  .lottie {
    position: absolute;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
</style>

<div class="lottie">
  <LottiePlayer
    src="https://assets7.lottiefiles.com/temp/lf20_0VFaxl.json"
    autoplay={true}
    loop={false}
    controls={false}
    renderer="svg"
    background="transparent"
    height={500}
    width={500}
    {controlsLayout} />
</div>
