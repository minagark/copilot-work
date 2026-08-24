---
layout: index
title: "Rock Paper Scissors"
heading: "Rock Paper Roshambo in JavaScript"
subheading: "Game History"
description: "Roshambo"
user-story: "As a user, I want to telegraph my play to the bot to play mind games with it."
---

Which one will it be?
<a href="#" onclick="playRoshambo('rock')">rock</a>
<a href="#" onclick="playRoshambo('paper')">paper</a>
<a href="#" onclick="playRoshambo('scissors')">scissors</a>
<input value="" type="text" id="userInput" placeholder="Enter your choice (rock, paper, scissors)"/>
<div id="results"></div>
<br/>
<div id="history"></div>

<script>
games = JSON.parse(localStorage.getItem('games')) || [];
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
    } else {
        result = "lose";
    }
    
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
