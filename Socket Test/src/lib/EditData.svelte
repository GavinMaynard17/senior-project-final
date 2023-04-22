<script>
  import { newSocket, dbMsg } from "./socket";
  import { databaseStore } from "./stores";

  let database;

  export let index = 0;

  let old_ign;
  let ign;
  let firstName;
  let lastName;
  let games_played;
  let goals;
  let shots;
  let assists;
  let saves;
  let demos;

  if ($databaseStore) {
    database = $databaseStore;
    old_ign = database[index].ign;
    ign = database[index].ign;
    firstName = database[index].first_name;
    lastName = database[index].last_name;
    games_played = database[index].games_played;
    goals = database[index].goals;
    shots = database[index].shots;
    assists = database[index].assists;
    saves = database[index].saves;
    demos = database[index].demos;
  }

  function updateData() {
    newSocket.send(
      JSON.stringify({
        receiver: "Server",
        event: "edit_player",
        data: {
          old_ign: old_ign,
          ign: ign,
          first_name: firstName,
          last_name: lastName,
          games_played: games_played,
          goals: goals,
          shots: shots,
          assists: assists,
          saves: saves,
          demos: demos,
        },
      })
    );

    newSocket.send(JSON.stringify(dbMsg));
  }
</script>

<a href="#/database_edit">Go back</a>

<form>
  <p>Gamertag</p>
  <input
    id="input"
    type="text"
    bind:value={ign}
    placeholder="Gamertag"
    required
  />
  <p>First Name</p>
  <input
    id="input"
    type="text"
    bind:value={firstName}
    placeholder="First Name"
    required
  />
  <p>Last Name</p>
  <input
    id="input"
    type="text"
    bind:value={lastName}
    placeholder="Last Name"
    required
  />
  <p>Total Games Played</p>
  <input
    id="input"
    type="text"
    bind:value={games_played}
    placeholder="Total number of games"
    required
  />
  <p>Total Goals</p>
  <input
    id="input"
    type="text"
    bind:value={goals}
    placeholder="Total number of goals"
    required
  />
  <p>Total Shots</p>
  <input
    id="input"
    type="text"
    bind:value={shots}
    placeholder="Total number of shots"
    required
  />
  <p>Total Assists</p>
  <input
    id="input"
    type="text"
    bind:value={assists}
    placeholder="Total number of assists"
    required
  />
  <p>Total Saves</p>
  <input
    id="input"
    type="text"
    bind:value={saves}
    placeholder="Total number of saves"
    required
  />
  <p>Total Demos</p>
  <input
    id="input"
    type="text"
    bind:value={demos}
    placeholder="Total number of demos"
    required
  />
  <button type="submit" on:click={updateData}
    ><a href="#/database_edit" style="color: black; text-decoration: none;"
      >Update</a
    ></button
  >
</form>

<style>
  form {
    display: flex;
    flex-direction: column;
  }

  input {
    width: 250px;
  }

  button {
    margin-top: 10px;
    width: 100px;
  }
</style>
