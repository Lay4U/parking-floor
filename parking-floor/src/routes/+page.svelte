<script>
    import {onMount} from 'svelte';

    let startFloor = -20;
    let endFloor = 20;
    let selectedFloor = null;

    $:{
        console.log(startFloor);
        console.log(endFloor);
        console.log(selectedFloor);
    }

    function selectFloor(floor) {
        selectedFloor = floor;
        if (typeof window !== 'undefined') {
            localStorage.setItem('selectedFloor', floor);
        }
    }

    onMount(() => {
        if (typeof window !== 'undefined') {
            selectedFloor = localStorage.getItem('selectedFloor');
        }
    });
</script>

<div>
  <label>
    시작 층:
    <select bind:value={startFloor}>
      {#each Array(41) as _, i}
        <option>{i - 20}</option>
      {/each}
    </select>
  </label>
  <label>
    끝 층:
    <select bind:value={endFloor}>
      {#each Array(41) as _, i}
        <option>{i - 20}</option>
      {/each}
    </select>
  </label>
</div>

<div>
  {#each Array(endFloor - startFloor + 1) as _, i (i)}
    <button on:click={() => selectFloor(i + startFloor)}>
      {i + startFloor > 0 ? `${i + startFloor} 층` : i + startFloor < 0 ? `B${Math.abs(i + startFloor)} 층` : '기타'}
    </button>
  {/each}
</div>

<p>선택된 층: {selectedFloor > 0 ? `${selectedFloor} 층` : selectedFloor < 0 ? `B${Math.abs(selectedFloor)} 층` : '기타'}</p>
