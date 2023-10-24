<script>
  const getFromLocalStorage = (key, defaultValue) =>
    typeof window !== 'undefined' && localStorage.getItem(key) !== null
      ? Number(localStorage.getItem(key))
      : defaultValue;

  const setToLocalStorage = (key, value) =>
    typeof window !== 'undefined' && localStorage.setItem(key, value.toString());

  const updateLocalStorage = ({startFloor, endFloor, parkedDate}) => {
    setToLocalStorage('startFloor', startFloor);
    setToLocalStorage('endFloor', endFloor);
    if(parkedDate) setToLocalStorage('parkedDate', parkedDate);
  };

  let floorsState = {
    startFloor: getFromLocalStorage('startFloor', -20),
    endFloor: getFromLocalStorage('endFloor', 20),
    selectedFloor: getFromLocalStorage('selectedFloor', '기타'),
    parkedDate: getFromLocalStorage('parkedDate', null),
  };

  const selectFloor = floor => {
    floorsState = {...floorsState, selectedFloor: floor};
    setToLocalStorage('selectedFloor', floor);
    updateParkedDate();
  };

  const updateParkedDate = () => {
    const currentDate = new Date().toLocaleString();
    floorsState = {...floorsState, parkedDate: currentDate};
    setToLocalStorage('parkedDate', currentDate);
  };

  $: updateLocalStorage(floorsState);

</script>

<div class="container">
    <div class="main">주차된
        층: {floorsState.selectedFloor > 0 ? `${floorsState.selectedFloor} 층` : floorsState.selectedFloor < 0 ? `B${Math.abs(floorsState.selectedFloor)} 층` : '기타'}</div>
  <div class="parked-date">
    주차 날짜: {floorsState.parkedDate ? floorsState.parkedDate : 'N/A'}
  </div>
    <div>
        <label>
            시작 층:
            <select bind:value={floorsState.startFloor}>
                {#each Array(41) as _, i}
                    <option value={i - 20}>{i - 20}</option>
                {/each}
            </select>
        </label>
        <label>
            끝 층:
            <select bind:value={floorsState.endFloor}>
                {#each Array(41) as _, i}
                    <option value={20 - i}>{20 - i}</option>
                {/each}
            </select>
        </label>
    </div>

    {#if floorsState.selectedFloor !== null && floorsState.endFloor > floorsState.startFloor}
        <div>
            {#each Array(floorsState.endFloor - floorsState.startFloor + 1) as _, i (i)}
                <button
                        class:selected={floorsState.selectedFloor == i + floorsState.startFloor}
                        class="button"
                        on:click={() => selectFloor(i + floorsState.startFloor)}
                >
                    {i + floorsState.startFloor > 0 ? `${i + floorsState.startFloor} 층` : i + floorsState.startFloor < 0 ? `B${Math.abs(i + floorsState.startFloor)} 층` : '기타'}
                </button>
            {/each}
        </div>
    {/if}

    <p><a href="https://github.com/Lay4U/parking-floor">소스코드</a></p>
</div>


<style>
    .button {
        display: inline-block;
        width: calc(100% / 3 - 20px);
        height: 4rem;
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
        background: linear-gradient(
                to bottom,
                rgba(8, 77, 126, 0) 0%,
                rgba(8, 77, 126, .42) 100%
        ),
        linear-gradient(
                to right,
                rgba(184, 238, 255, .32),
                rgba(184, 238, 255, .32)
        );
        z-index: -1;
    }

    .button::after {
        content: "";
        display: block;
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: linear-gradient(
                to bottom,
                rgba(8, 77, 126, 0) 0%,
                rgba(8, 77, 126, .42) 100%
        ),
        linear-gradient(
                to right,
                rgba(184, 238, 255, .32),
                rgba(184, 238, 255, .32)
        );
        border-radius: .5rem;
        opacity: .6;
        z-index: -1;
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

    .container {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        font-size: 20px;
        background: #F5F5F5
    }

    select {
        width: 5rem;
        height: 2rem;
        border-radius: 0.5rem;
        border: none;
        background-color: chocolate;
        color: #ffffff;
        font-size: 1rem;
        font-weight: 400;
        text-align: center;
        cursor: pointer;
        transition: transform .3s ease-in-out;
    }

</style>
