<script>
  let players = [
    {
      id: 1,
      name: "Abhishek",
      rating: 1500
    },
    {
      id: 2,
      name: "Rahul",
      rating: 1600
    }
  ];

  let matches = [];

  function createMatch() {
    if (players.length < 2) {
      alert("At least two players are required");
      return;
    }

    const shuffledPlayers = [...players].sort(
      () => Math.random() - 0.5
    );

    const newMatch = {
      id: Date.now(),
      playerOne: shuffledPlayers[0],
      playerTwo: shuffledPlayers[1],
      winner: null,
      status: "Pending"
    };

    matches = [...matches, newMatch];
  }

  function selectWinner(matchId, winnerId) {
    matches = matches.map((match) => {
      if (match.id === matchId) {
        const winner =
          match.playerOne.id === winnerId
            ? match.playerOne
            : match.playerTwo;

        return {
          ...match,
          winner,
          status: "Completed"
        };
      }

      return match;
    });
  }
</script>

<div class="match-page">

  <h2>Match Management</h2>

  <button on:click={createMatch}>
    Create Random Match
  </button>

  {#each matches as match}

    <div class="match-card">

      <h3>
        {match.playerOne.name}
        vs
        {match.playerTwo.name}
      </h3>

      <p>Status: {match.status}</p>

      {#if match.winner}

        <p>
          Winner: {match.winner.name}
        </p>

      {:else}

        <button
          on:click={() =>
            selectWinner(match.id, match.playerOne.id)
          }
        >
          {match.playerOne.name} Wins
        </button>

        <button
          on:click={() =>
            selectWinner(match.id, match.playerTwo.id)
          }
        >
          {match.playerTwo.name} Wins
        </button>

      {/if}

    </div>

  {/each}

</div>