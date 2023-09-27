<script>
    let startFloor = typeof window !== 'undefined' && localStorage.getItem('startFloor') !== null ? Number(localStorage.getItem('startFloor')) : -20;
    let endFloor = typeof window !== 'undefined' && localStorage.getItem('endFloor') !== null ? Number(localStorage.getItem('endFloor')) : 20;
    let selectedFloor = typeof window !== 'undefined' && localStorage.getItem('selectedFloor') !== null ? Number(localStorage.getItem('selectedFloor')) : '기타';

    function selectFloor(floor) {
        selectedFloor = floor;
        if (typeof window !== 'undefined') {
            localStorage.setItem('selectedFloor', floor.toString());
        }
    }

    $: {
        if (typeof window !== 'undefined') {
            console.log('hi');
            localStorage.setItem('startFloor', startFloor.toString());
            localStorage.setItem('endFloor', endFloor.toString());
        }
    }

    $:console.log(startFloor, endFloor);

</script>


<div class="main">주차된 층: {selectedFloor > 0 ? `${selectedFloor} 층` : selectedFloor < 0 ? `B${Math.abs(selectedFloor)} 층` : '기타'}</div>

<div>
  <label>
    시작 층:
    <select bind:value={startFloor}>
      {#each Array(41) as _, i}
        <option value={i - 20}>{i - 20}</option>
      {/each}
    </select>
  </label>
  <label>
    끝 층:
    <select bind:value={endFloor}>
      {#each Array(41) as _, i}
        <option value={i - 20}>{i - 20}</option>
      {/each}
    </select>
  </label>
</div>

{#if selectedFloor !== null && endFloor > startFloor}
  <div>
    {#each Array(endFloor - startFloor + 1) as _, i (i)}
      <button class:selected={selectedFloor == i + startFloor} class="button" on:click={() => selectFloor(i + startFloor)}>
        {i + startFloor > 0 ? `${i + startFloor} 층` : i + startFloor < 0 ? `B${Math.abs(i + startFloor)} 층` : '기타'}
      </button>
    {/each}
  </div>
{/if}



<style>
    .button {
        display: inline-block;
        width: calc(100% / 3 - 20px);
        margin: 10px;
        padding: 1rem 2rem;
        border-radius: .5rem;
        border: none;
        font-size: 1rem;
        font-weight: 400;
        color: #000000;
        text-align: center;
        backdrop-filter: blur(10px);
        cursor: pointer;
        transition: transform .3s ease-in-out;
    }

    .button.selected {
        background-color: #8c77ff;
        color: #000000;
        transform: scale(1.05);
    }

    .button::before {
        content: "";
        display: block;
        position: absolute;
        left: 0;
        top: 0;
        height: 100%;
        width: 100%;
        border-radius: .5rem;
        background:
                linear-gradient(
                        to bottom,
                        rgba(8,77,126,0)0%,
                        rgba(8,77,126,.42)100%
                ),
                linear-gradient(
                        to right,
                        rgba(184,238,255,.32),
                        rgba(184,238,255,.32)
                );
        z-index:-1;
    }

    .button::after {
        content:"";
        display:block;
        position:absolute;
        top:0;
        left:0;
        width:100%;
        height:100%;
        background:
                linear-gradient(
                        to bottom,
                        rgba(8,77,126,0)0%,
                        rgba(8,77,126,.42)100%
                ),
                linear-gradient(
                        to right,
                        rgba(184,238,255,.32),
                        rgba(184,238,255,.32)
                );
        border-radius:.5rem;
        opacity:.6;
        z-index:-1;
    }

    .button:hover {
        transform: scale(1.05);
    }

    @media (max-width: 600px) {
        .button {
            width: calc(100% / 2 - 20px);
        }
    }

    .button:active, .button:focus {
        outline: none;
        box-shadow: 0 0 10px #8c77ff;
    }

    .button {
        transition: all 0.3s ease-in-out;
    }

    .button:hover {
        transform: scale(1.05);
    }

</style>
