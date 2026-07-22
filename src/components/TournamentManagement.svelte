<script>
  let tournaments = [];

  let tournamentName = "";
  let tournamentDate = "";
  let tournamentLocation = "";

  function addTournament() {
    if (
      tournamentName === "" ||
      tournamentDate === "" ||
      tournamentLocation === ""
    ) {
      alert("Please fill all fields");
      return;
    }

    const newTournament = {
      id: Date.now(),
      name: tournamentName,
      date: tournamentDate,
      location: tournamentLocation,
      players: []
    };

    tournaments = [...tournaments, newTournament];

    tournamentName = "";
    tournamentDate = "";
    tournamentLocation = "";
  }

  function deleteTournament(id) {
    tournaments = tournaments.filter(
      (tournament) => tournament.id !== id
    );
  }
</script>

<div class="tournament-page">

  <h2>Tournament Management</h2>

  <div class="tournament-form">

    <input
      type="text"
      placeholder="Tournament name"
      bind:value={tournamentName}
    />

    <input
      type="date"
      bind:value={tournamentDate}
    />

    <input
      type="text"
      placeholder="Location"
      bind:value={tournamentLocation}
    />

    <button on:click={addTournament}>
      Create Tournament
    </button>

  </div>

  <div class="tournament-list">

    {#each tournaments as tournament}

      <div class="tournament-card">

        <h3>{tournament.name}</h3>

        <p>Date: {tournament.date}</p>

        <p>Location: {tournament.location}</p>

        <p>
          Players: {tournament.players.length}
        </p>

        <button
          on:click={() => deleteTournament(tournament.id)}
        >
          Delete Tournament
        </button>

      </div>

    {/each}

  </div>

</div>