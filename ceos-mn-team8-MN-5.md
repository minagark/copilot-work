---
layout: ceos-mn-index
title: "Rock Paper Scissors"
heading: "Rock Paper Roshambo in JavaScript"
subheading: "Game History"
description: "Roshambo"
user-story: "As a user, I can play a best 2 out of 3 against the bot so that the game doesn't end too fast."
---

Which one will it be?
<a href="#" onclick="playRoshambo('rock')">rock</a>
<a href="#" onclick="playRoshambo('paper')">paper</a>
<a href="#" onclick="playRoshambo('scissors')">scissors</a>
<div id="results"></div>
<div id="results2"></div>
<br/>
<div id="history"></div>

<script>
games = JSON.parse(localStorage.getItem('games')) || [];
wins = 0;
losses = 0;

playRoshambo = function(clientGesture){
    possibleGestures = ["rock", "paper", "scissors"];
    serverGesture = possibleGestures[Math.floor(Math.random() * 3)]; 
    
    if (clientGesture === serverGesture) {
        result = "tie";
    } else if (
        (clientGesture === "rock" && serverGesture === "scissors") ||
        (clientGesture === "paper" && serverGesture === "rock") ||
        (clientGesture === "scissors" && serverGesture === "paper")
    ) {
        result = "win";
        wins += 1;
    } else {
        result = "lose";
        losses += 1;
    }
    
    if (wins >= 2) {
        result2 = "You won the best 2 out of 3!";
        wins = 0;
        losses = 0;
    } else if (losses >= 2) {
        result2 = "You lost the best 2 out of 3!";
        wins = 0;
        losses = 0;
    } else {
        result2 = "";
    }
    document.getElementById('results2').innerHTML = result2;

    document.getElementById('results').innerHTML = result;
    
    saveGame(clientGesture, serverGesture, result);
    showHistory();
}

saveGame = function(clientGesture, serverGesture, result) {
    game = {
        result: result,
        serverGesture: serverGesture,
        clientGesture: clientGesture,
        time: new Date()
    };
    games.push(game);
    localStorage.setItem('games', JSON.stringify(games));
    showHistory();
}

showHistory = function() {
    historyText = "";
    for (game of games) {
        historyText += "<a href='#' onclick=\"deleteGame('" + game.time + "')\">delete</a>";
        historyText += "<div>";
        historyText += game.clientGesture + " | ";
        historyText += game.serverGesture + " | ";
        historyText += game.result + " | ";
        historyText += game.time + " | ";
        historyText += "</div>";
    }
    document.getElementById('history').innerHTML = historyText;
}

deleteGame = function(time) {
    games = games.filter(game => game.time != time);
    localStorage.setItem('games', JSON.stringify(games));
    showHistory();
}

</script>
