---
layout: default
title: Student Blog
permalink: /tracker
---


<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Daily Instrument Practice Tracker</title>
    <style>
       body {
            background-image: url({{site.baseurl}}/images/instrument_gifbg.gif);;
            background-size: contain;
            background-repeat: no-repeat;
            background-attachment: local;
            font-family: 'Segoe UI', sans-serif;
        }
        .container {
            text-align: center;
            padding: 50px;
            background-color: rgb(183, 255, 217);
            border-radius: 10px;
            margin: 50px auto;
            max-width: 400px;
        }
        h1 {
            color: #333;
        }
        #study-time {
            font-size: 24px;
            padding: 10px;
            width: 100%;
            border: none;
            text-align: center;
        }
        #save-button {
            background-color: #8257B4;
            color: #fff;
            border: none;
            padding: 10px 20px;
            cursor: pointer;
        }
        /* Style for the weekly instrument practice log */
        #weekly-log {
            text-align: left;
            margin-top: 20px;
            padding: 10px;
            background-color: #fff;
            border-radius: 5px;
            box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.3);
        }
        #weekly-log table {
            width: 100%;
            border-collapse: collapse;
        }
        #weekly-log th, #weekly-log td {
            padding: 8px;
            border-bottom: 1px solid #ddd;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Name</h1>
        <input type="text" id="name" placeholder="Enter first name">
        <h1>Instrument</h1>
        <input type="text" id="instrument" placeholder="Enter instrument name">
        <h1>Number of Minutes Practiced</h1>
        <input type="number" id="practice-time" placeholder="Enter practice time (minutes)">
        <br><br>
        <button id="save-button">Save</button>
        <!-- Weekly Practice Log Display -->
        <div id="weekly-log">
            <h2>Weekly Instrument  Practice Time  Log</h2>
            <table>
                <thead>
                    <tr>
                        <th>Date</th>
                        <th>Practice Time (minutes)</th>
                    </tr>
                </thead>
                <tbody>
                    <!-- Practice log entries will be displayed here -->
                </tbody>
            </table>
        </div>
    </div>
    <!-- Relevant Links -->
    <div style="text-align: center; margin-top: 20px;">
        <a href="https://pianopower.org/16-benefits-of-playing-an-instrument/" target="_blank">Benefits of Practicing</a> |
        <a href="https://www.betterup.com/blog/15-ways-to-improve-your-focus-and-concentration-skills" target="_blank">How to Increase Concentration</a>
    </div>
    <script>
        // JavaScript to save practice time to local storage
        document.getElementById("save-button").addEventListener("click", function () {
            const practiceTime = document.getElementById("practice-time").value;
            if (practiceTime !== "") {
                const currentDate = new Date().toLocaleDateString();
                const practiceData = JSON.parse(localStorage.getItem("practiceData")) || {};
                practiceData[currentDate] = parseInt(practiceTime);
                localStorage.setItem("practiceData", JSON.stringify(practiceData));
                alert(`Practice time (${practiceTime} minutes) saved for ${currentDate}`);
                document.getElementById("practice-time").value = "";
                // Refresh the practice log display
                displayWeeklyLog();
            } else {
                alert("Please enter a valid practice time.");
            }
        });
  // Function to display the weekly practice log
        function displayWeeklyLog() {
            const practiceData = JSON.parse(localStorage.getItem("practiceData")) || {};
            const tableBody = document.querySelector("#weekly-log table tbody");
            tableBody.innerHTML = "";
            for (const date in practiceData) {
                const row = tableBody.insertRow();
                const cellDate = row.insertCell(0);
                const cellTime = row.insertCell(1);
                cellDate.textContent = date;
                cellTime.textContent = practiceData[date];
            }
        }
//Call the function to display the weekly practice log when the page loads
        displayWeeklyLog();
    </script>

</body>
</html>



