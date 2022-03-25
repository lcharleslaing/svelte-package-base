<script>
    import { onMount } from "svelte";

    let hideClock = true;
    export let width = 100;
    export let card = true;
    export let cardColor = "";
    export let textSize;

    let time = new Date();

    // these automatically update when `time`
    // changes, because of the `$:` prefix
    $: month = time.getMonth();
    $: mytime = time.getTime();
    $: day = time.getDate();
    $: year = time.getFullYear();
    $: hours = time.getHours();
    $: formatted_hours = hours > 12 ? time.getHours() - 12 : time.getHours();
    $: minutes = minutes < 10 ? `0${time.getMinutes()}` : time.getMinutes();
    $: formatted_minutes =
        minutes < 10 ? `0${time.getMinutes()}` : time.getMinutes();
    $: seconds = time.getSeconds();
    $: formatted_seconds =
        seconds < 10 ? `0${time.getSeconds()}` : time.getSeconds();
    $: A = hours < 12 ? "AM" : "PM";
    $: currentTime = `${formatted_hours}:${formatted_minutes}:${formatted_seconds} ${A}`;
    $: currentTimeNoSeconds = `${formatted_hours}:${formatted_minutes} ${A}`;
    $: currentDate = `${month + 1}/${day}/${year}`;

    $: onMount(() => {
        const interval = setInterval(() => {
            time = new Date();
        }, 1000);

        return () => {
            clearInterval(interval);
        };
    });

    const toggleClock = () => {
        if (hideClock) {
            hideClock = false;
        } else {
            hideClock = true;
        }
    };
</script>

<div
    style="width: {width}px; font-size: {textSize}px"
    class:hidden={hideClock}
    class:card={card === true}
    class="clock {cardColor}"
>
    <!-- Show Clock -->
    <div
        on:click={toggleClock}
        class:hidden={!hideClock}
        class="text-center cursor-pointer"
    >
        {currentTimeNoSeconds}
    </div>
    <!-- Hide Clock -->
    <div
        on:click={toggleClock}
        class:hidden={hideClock}
        class=" cursor-pointer text-center"
    >
        Hide
    </div>
    <div class:hidden={hideClock} class="">
        <svg viewBox="-50 -50 100 100">
            <circle class="clock-face" r="48" />

            <!-- markers -->
            {#each [0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55] as minute}
                <line
                    class="major"
                    y1="35"
                    y2="45"
                    transform="rotate({30 * minute})"
                />

                {#each [1, 2, 3, 4] as offset}
                    <line
                        class="minor"
                        y1="42"
                        y2="45"
                        transform="rotate({6 * (minute + offset)})"
                    />
                {/each}
            {/each}

            <!-- hour hand -->
            <line
                class="hour"
                y1="2"
                y2="-20"
                transform="rotate({30 * hours + minutes / 2})"
            />

            <!-- minute hand -->
            <line
                class="minute"
                y1="4"
                y2="-30"
                transform="rotate({6 * minutes + seconds / 10})"
            />

            <!-- second hand -->
            <g transform="rotate({6 * seconds})">
                <line class="second" y1="10" y2="-38" />
                <line class="second-counterweight" y1="10" y2="2" />
            </g>
        </svg>
    </div>
    <p class:hidden={hideClock}>{currentTime}</p>
    <p class:hidden={hideClock}>{currentDate}</p>
</div>

<!--   shadow-md text-xs -->
<style>
    :root {
        --card-color: "red";
    }

    svg {
        width: 100%;
        height: 100%;
    }

    .clock-face {
        stroke: #333;
        fill: white;
    }

    .minor {
        stroke: #999;
        stroke-width: 0.5;
    }

    .major {
        stroke: #333;
        stroke-width: 1;
    }

    .hour {
        stroke: #333;
    }

    .minute {
        stroke: #666;
    }

    .second,
    .second-counterweight {
        stroke: rgb(180, 0, 0);
    }

    .second-counterweight {
        stroke-width: 3;
    }

    .clock {
        display: grid;
        place-items: center;
        width: calc(width);
        font-size: 12px;
    }

    .absolute-br {
        position: absolute;
        bottom: 0.5em;
        right: 0.5em;
    }

    .card {
        padding: 0.25em;
        border-radius: .75em;
        box-shadow: 0px 1px 2px rgba(0, 0, 0, 0.8);
    }
</style>
