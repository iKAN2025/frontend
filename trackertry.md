<!-- put your HTML code in this cell, Make sure to press the Run button to see your results below -->

<!-- put your HTML code in this cell, Make sure to press the Run button to see your results below -->
<!-- put your HTML code in this cell, Make sure to press the Run button to see your results below -->
<html lang="en">
<head>
 <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Daily Instrument Practice Tracker</title>
    <style>
       <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 20px;
            background-color:white;
        }
         header {
            background-color: #ffb6c1;
            color: #fff;
            padding: 10px;
            text-align: center;
        }
        h1 {
            margin: 0;
        }
        section {
            max-width: 800px;
            margin: 20px auto;
            padding: 20px;
            background-color:white;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
            border-radius: 10px;
            overflow: hidden;
        }
        footer {
            background-color: #333;
            color: #fff;
            text-align: center;
            padding: 10px;
            position: fixed;
            width: 100%;
            bottom: 0;
        }
    </style>

<head>
 <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Daily Instrument Practice Tracker</title>
    <style>
        body {
            background-image: url({{site.baseurl}}/images/celloplaying.gif);;
            background-size: contain;
            background-repeat: no-repeat;
            background-attachment: fixed;
            font-family: 'Segoe UI', sans-serif;
        }
        .container {
            text-align: center;
            padding: 50px;
            background-color: rgb(255, 182, 193);
            border-radius: 10px;
            margin: 50px auto;
            max-width: 600px;
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
        .dropdown {
            display: inline-block;
            width: 100%;
        }
        .dropdown select {
            width: 100%;
            padding: 10px;
        }

</style>
</head>
<body>
    <div class="container">
        <h1>Name</h1>
        <div class="dropdown">
            <select id="name">
                <option value="">Select a Name</option>
                <!-- Populate dropdown options dynamically using JavaScript -->
            </select>
        </div>
        <h1>Instrument</h1>
        <input type="text" id="instrument" placeholder="Enter instrument name">
        <h1>Number of Minutes Practiced</h1>
        <input type="number" id="practice-time" placeholder="Enter practice time (minutes)">
        <br><br>
        <button id="save-button">Save</button>
        <!-- Weekly Practice Log Display -->
        <div id="weekly-log">
            <h2>Weekly Practice Log</h2>
            <table>
                <thead>
                    <tr>
                        <th>Name</th>
                        <th>Date</th>
                        <th>Instrument</th>
                        <th>Practice Time (minutes)</th>
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
        <a href="https://pianopower.org/16-benefits-of-playing-an-instrument/" target="_blank">Benefits of Practicing</a> |
        <a href="https://www.betterup.com/blog/15-ways-to-improve-your-focus-and-concentration-skills" target="_blank">How to Increase Concentration</a>
    </div>
<script>
    function populateNameDropdown() {
        // Make an API request to fetch user names
        // Replace this with your actual API endpoint
        fetch('http://127.0.0.1:8240/api/users/')
            .then(response => {
                if (!response.ok) {
                    throw new Error('Network response was not ok');
                }
                return response.json();
            })
            .then(data => {
                const nameDropdown = document.getElementById('name');
                data.forEach(user => {
                    const option = document.createElement('option');
                    option.value = user.id;
                    option.textContent = user.name;
                    nameDropdown.appendChild(option);
                });
            })
            .catch(error => {
                console.error('Error fetching user names:', error);
            });
    }
    populateNameDropdown()
   function updateTracking() {
    const selectedUserId = document.getElementById('name').value;
    const name = document.getElementById('name').options[document.getElementById('name').selectedIndex].text;
    const instrument = document.getElementById('instrument').value;
    const practiceTime = document.getElementById('practice-time').value;
    const currentDate = new Date().toLocaleDateString();
    if (practiceTime !== "" && name !== "" && instrument !== "") {
        fetch(`http://127.0.0.1:8240/api/users/${selectedUserId}`) //comment
            .then(response => {
                if (!response.ok) {
                    throw new Error('Network response was not ok');
                }
                return response.json();
            })
            .then(data => {
                // Combine old and new tracking data
                const originalTrackingData = Array.isArray(data.tracking) ? data.tracking : [];
                const newTrackingData = {
                    userName: name,
                    instrumentName: instrument,
                    practiceDate: currentDate,
                    practiceTime: practiceTime
                };
                const updatedTrackingData = [...originalTrackingData, newTrackingData];
                const data2 = {
                    "id": selectedUserId,
                    "name": name,
                    "uid": "life",
                    "dob": "10/12/13",
                    "age": "16",
                    "tracking": updatedTrackingData // Include updated tracking data
                };
                var jsonData = JSON.stringify(data2);
                fetch(`http://127.0.0.1:8240/api/users/${selectedUserId}`, {
                    method: 'PUT',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: jsonData
                }) //get method not working
                    .then(response => {
                        if (!response.ok) {
                            console.log('Network response was not ok');
                        }
                        return response.json();
                    })
                    .then(data => {
                        console.log('User tracking data updated:', data);
                    })
                    .catch(error => {
                        console.error('Error updating user tracking data:', error);
                    });
            })
            .catch(error => {
                console.error('Error fetching user data:', error);
            });
    } else {
        alert("Please enter a valid practice time, name, and instrument.");
    }
}
const url = "http://127.0.0.1:8240/api/users/"   
function displayWeeklyLog() {
    fetch(url) // Replace 'url' with the actual API endpoint
        .then(response => response.json())
        .then(data => {
            const tableBody = document.querySelector("#weekly-log table tbody");
            tableBody.innerHTML = ""; // Clear the existing table data
            data.forEach(user => {
                if (Array.isArray(user.tracking)) {
                    user.tracking.forEach(trackingItem => {
                        const tracking = parseTracking(trackingItem);
                        const row = tableBody.insertRow();
                        const nameCell = row.insertCell(0);
                        const dobCell = row.insertCell(1);
                        const practicedateCell = row.insertCell(2);
                        const instrumentCell = row.insertCell(3);
                        const practicetimeCell = row.insertCell(4);
                        nameCell.textContent = user.name;
                        dobCell.textContent = user.dob;
                        practicedateCell.textContent = tracking.practiceDate;
                        instrumentCell.textContent = tracking.instrumentName;
                        practicetimeCell.textContent = tracking.practiceTime;
                    });
                }
            });
        })
        .catch(error => {
            console.error('Error:', error);
            alert(error);
        });
}
function parseTracking(tracking) {
    try {
        return JSON.parse(tracking);
    } catch (e) {
        return tracking;
    }
}
        displayWeeklyLog();
document.addEventListener('DOMContentLoaded', function () {
    document.getElementById('save-button').addEventListener('click', function () {
        updateTracking();
    });
});
