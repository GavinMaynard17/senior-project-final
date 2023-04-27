<script>
    import { socketMessageStore, newSocket, dbMsg } from "./socket";
    import { gameDataStore, playerNameStore, databaseStore, databaseNames } from "./stores";

    $: database = $databaseStore
    $: allData = $gameDataStore
    $: allPlayers = $playerNameStore 
    
    $: userInputs = []
    
    $: storedPlayers = $databaseNames

    function wipeData(){
        gameDataStore.set([])
        playerNameStore.set([])
        socketMessageStore.set({
            event: "default",
            data: {},
        })
    }
    
    function submitData(){

        userInputs.forEach((input, i) => {
            if (input != -1){
                
                allData.forEach(gameData => { gameData.forEach(playerData => { 
                    if(playerData.name == allPlayers[i]){
                        console.log('index', input);
                        console.log("selected player", allPlayers[i])
                        console.log("selected database", database)
                        updateData(input, playerData)
                    }
                })})
            }
        })
        wipeData() //reset the gameDataStore
    }

    function updateData(index, newData){
        console.log("adding this data", newData)
        newSocket.send(JSON.stringify({
            receiver: "Server",
            event: "update_player",
            data: {
                ign: database[index].ign,
                games_played: 1,
                goals: newData.goals,
                shots: newData.shots,
                assists: newData.assists,
                saves: newData.saves,
                demos: newData.demos
            }
        }))

        // newSocket.send(JSON.stringify(dbMsg))
    }

    
</script>
<body>
    

<h1>Add player to database</h1>

    {#each allPlayers as player, i}
        <div class="all-players">
            <select class="dropdown" bind:value={userInputs[i]}>
                <option value=-1></option>
                {#each storedPlayers as name, i}
                    <option value="{i}">{name}</option>
                {/each}
            </select>
            <p class="player">{player}</p>
        </div>
    {/each}

    {#if allPlayers.length == 0}
        <p>No data received</p>
    {/if}


    <a href="#/" >
        <button type="button" on:click={submitData}>Submit Data</button>
    </a>
</body>





<style>

    .all-players{
        display: flex;
        flex-direction: row;
    }

    .player{
        padding-left: 10px;
        margin-top: 3px;
        color: white;
        
    }

    .dropdown{
        height:25px
    }

    p{
        color: white;
    }

    h1{
        color: white;
    }
</style>