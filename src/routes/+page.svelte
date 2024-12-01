<script lang="ts">
  import { Slider } from "$lib/components/ui/slider";
  import { Button } from "$lib/components/ui/button";
  import { Play, Pause } from 'lucide-svelte';
  import { onMount } from 'svelte';

  let isPlaying = $state(false);
  let bpm = $state(120);
  let audioContext: AudioContext;
  let clickSound: AudioBuffer;
  let intervalId: number;

  onMount(async () => {
    try {
      audioContext = new AudioContext();
      const response = await fetch('/click.mp3');
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      const arrayBuffer = await response.arrayBuffer();
      clickSound = await audioContext.decodeAudioData(arrayBuffer);
    } catch (error) {
      console.error('Failed to load audio:', error);
    }
  });

  function playClick() {
    const source = audioContext.createBufferSource();
    source.buffer = clickSound;
    source.connect(audioContext.destination);
    source.start();
  }

  function toggleMetronome() {
    if (isPlaying) {
      clearInterval(intervalId);
      isPlaying = false;
    } else {
      const interval = (60 / bpm) * 1000;
      intervalId = setInterval(playClick, interval);
      isPlaying = true;
    }
  }

  function handleBpmChange(value: number[]) {
    bpm = value[0];
    if (isPlaying) {
      clearInterval(intervalId);
      const interval = (60 / bpm) * 1000;
      intervalId = setInterval(playClick, interval);
    }
  }
</script>

<div class="flex flex-col items-center justify-center min-h-screen gap-8 p-4">
  <div class="flex flex-col items-center gap-4 w-full max-w-md">
    <div class="text-2xl font-semibold">{bpm} BPM</div>
    
    <div class="w-full px-4">
      <Slider
        value={[bpm]}
        onValueChange={handleBpmChange}
        min={40}
        max={208}
        step={1}
        class="w-full"
      />
    </div>

    <Button
      on:click={toggleMetronome}
      variant="default"
      class="w-32 h-32 rounded-full"
    >
      {#if isPlaying}
        <Pause class="w-12 h-12" />
      {:else}
        <Play class="w-12 h-12" />
      {/if}
    </Button>
  </div>
</div>
  