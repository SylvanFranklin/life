<script lang="ts">
    let painting = false;
    let size = 40;

    let board = Array(size)
        .fill(0)
        .map(() => Array(size).fill(0));

    function get_live_nieghbors(x: number, y: number) {
        // check if we are on the edge, for each and don't index out of bounds

        let live_neighbors = 0;
        const chord_pairs = [
            [x - 1, y - 1],
            [x - 1, y],
            [x - 1, y + 1],
            [x, y - 1],
            [x, y + 1],
            [x + 1, y - 1],
            [x + 1, y],
            [x + 1, y + 1],
        ];

        for (const [x, y] of chord_pairs) {
            if (board[x] && board[x][y] === 1) {
                live_neighbors++;
            }
        }
        return live_neighbors;
    }

    let running = false;
    function next_generation() {
        const new_board = structuredClone(board).map((row, x) =>
            row.map((cell: number, y: number) => {
                const live_neighbors = get_live_nieghbors(x, y);
                if (cell === 1) {
                    if (live_neighbors < 2 || live_neighbors > 3) {
                        return 0;
                    }
                    return 1;
                } else {
                    if (live_neighbors === 3) {
                        return 1;
                    }
                    return 0;
                }
            }),
        );

        board = new_board;

        if (running) {
            setTimeout(next_generation, 100);
        }
    }
</script>

<main
    class="w-full mx-auto items-center justify-center h-screen flex bg-gray-50 flex-col gap-4"
>
    <span class="flex flex-row gap-4">
        <button
            class={`${running ? "bg-red-400" : "bg-green-400"} text-white font-mono text-xl rounded-lg p-4 shadow-md font-bold`}
            on:click={() => {
                running = !running;
                next_generation();
            }}
        >
            {running ? "Stop" : "Start"}
        </button>
        <button
            class={`text-white bg-orange-400 font-mono text-xl rounded-lg p-4 shadow-md font-bold`}
            on:click={() => {
                running = false;
                board = Array(size)
                    .fill(0)
                    .map(() => Array(size).fill(0));
            }}
        >
            Clear
        </button>
    </span>
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <!-- svelte-ignore a11y-no-static-element-interactions -->
    <div
        class="rounded-lg overflow-clip shadow-lg"
        style={`display: grid; grid-template-columns: repeat(${size}, 1fr); grid-auto-rows: minmax(0, auto); `}
        on:mousedown={() => (painting = !painting)}
        on:mouseup={() => (painting = !painting)}
    >
        {#each board as row, i}
            {#each row as cell, j}
                <button
                    on:mouseenter={() =>
                        painting && (board[i][j] = cell == 0 ? 1 : 0)}
                    on:click={() => (board[i][j] = cell == 0 ? 1 : 0)}
                    class={`${cell == 0 ? "bg-black/80" : "bg-white"} shadow-md`}
                    style={`width: ${(4 * size) / 10}px; height: ${(4 * size) / 10}px;`}
                ></button>
            {/each}
        {/each}
    </div>
</main>

<!-- <style> -->
<!--     .custom-grid { -->
<!--         display: grid; -->
<!--         grid-template-columns: repeat(size, 1fr); -->
<!--         grid-auto-rows: minmax(0, auto); -->
<!--     } -->
<!-- </style> -->
