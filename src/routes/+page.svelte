<script>
    let board = [
        [0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0],
    ];

    function get_live_nieghbors(x, y) {
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

    function next_generation() {
        const new_board = board.map((row, x) =>
            row.map((cell, y) => {
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
    }
</script>

<main
    class="w-full mx-auto items-center justify-center h-screen flex bg-gray-50 flex-col gap-4"
>
    <button
        class="bg-green-400 text-white font-mono text-xl rounded-lg p-4 shadow-md font-bold"
        on:click={next_generation}
    >
        start
    </button>
    <div class="grid grid-rows-7 grid-cols-7 gap-2">
        {#each board as row}
            {#each row as cell}
                <button
                    on:click={() => (cell = cell == 0 ? 1 : 0)}
                    class={`w-20 h-20 ${cell == 0 ? "bg-black/80" : "bg-white"} rounded-md shadow-md`}
                ></button>
            {/each}
        {/each}
    </div>
</main>
