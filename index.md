---
layout: default
title: Student Blog
---



<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Daily Instrument Practice Tracker</title>
    <style>
        body {
            background-image: url({{site.baseurl}}/images/studying.png);;
            background-size: contain;
            background-repeat: no-repeat;
            background-attachment: fixed;
            font-family: 'Segoe UI', sans-serif;
        }
        .container {
            text-align: center;
            padding: 50px;
            background-color: rgb(239, 158, 208);
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
            background-color: #57A5F8;
            color: #fff;
            border: none;
            padding: 10px 20px;
            cursor: pointer;
        }
        /* Style for the weekly study log */
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
        <h1>Daily Study Time Tracker</h1>
        <input type="number" id="study-time" placeholder="Enter study time (minutes)">
        <br><br>
        <button id="save-button">Save</button>
        <!-- Weekly Study Log Display -->
        <div id="weekly-log">
            <h2>Weekly Study Log</h2>
            <table>
                <thead>
                    <tr>
                        <th>Date</th>
                        <th>Study Time (minutes)</th>
                    </tr>
                </thead>
                <tbody>
                    <!-- Study log entries will be displayed here -->
                </tbody>
            </table>
        </div>
    </div>
    <!-- Relevant Links -->
    <div style="text-align: center; margin-top: 20px;">
        <a href="https://studyworkgrow.com.au/2023/05/04/7-effective-study-techniques-for-high-school-students/" target="_blank">Study Tips</a> |
        <a href="https://www.betterup.com/blog/15-ways-to-improve-your-focus-and-concentration-skills" target="_blank">How to Increase Concentration</a>
    </div>
    <script>
        // JavaScript to save study time to local storage
        document.getElementById("save-button").addEventListener("click", function () {
            const studyTime = document.getElementById("study-time").value;
            if (studyTime !== "") {
                const currentDate = new Date().toLocaleDateString();
                const studyData = JSON.parse(localStorage.getItem("studyData")) || {};
                studyData[currentDate] = parseInt(studyTime);
                localStorage.setItem("studyData", JSON.stringify(studyData));
                alert(`Study time (${studyTime} minutes) saved for ${currentDate}`);
                document.getElementById("study-time").value = "";
                // Refresh the study log display
                displayWeeklyLog();
            } else {
                alert("Please enter a valid study time.");
            }
        });
        // Function to display the weekly study log
        function displayWeeklyLog() {
            const studyData = JSON.parse(localStorage.getItem("studyData")) || {};
            const tableBody = document.querySelector("#weekly-log table tbody");
            tableBody.innerHTML = "";
            for (const date in studyData) {
                const row = tableBody.insertRow();
                const cellDate = row.insertCell(0);
                const cellTime = row.insertCell(1);
                cellDate.textContent = date;
                cellTime.textContent = studyData[date];
            }
        }
        // Call the function to display the weekly log when the page loads
        displayWeeklyLog();
    </script>
</body>
</html>