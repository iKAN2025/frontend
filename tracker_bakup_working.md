<!-- put your HTML code in this cell, Make sure to press the Run button to see your results below -->

<!-- put your HTML code in this cell, Make sure to press the Run button to see your results below -->
<!-- put your HTML code in this cell, Make sure to press the Run button to see your results below -->
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Instruments Practice Tracker</title>
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

<html lang="en">
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
            padding: 50px;clea
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
            <h2>Weekly Practice Log</h2>
            <table>
                <thead>
                    <tr>
                        <th>Name5</th>
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
    <div id="api-response"></div>
    <!-- Relevant Links -->
    <div style="text-align: center; margin-top: 20px;">
        <a href="https://pianopower.org/16-benefits-of-playing-an-instrument/" target="_blank">Benefits of Practicing</a> |
        <a href="https://www.betterup.com/blog/15-ways-to-improve-your-focus-and-concentration-skills" target="_blank">How to Increase Concentration</a>
    </div>
    <script>
        // JavaScript to save practice time to local storage
          const resultContainer = document.getElementById("result");
  // prepare URL's to allow easy switch from deployment and localhost
  var url = "https://bella-flask-portfolio.stu.nighthawkcodingsociety.com/"
  // url = "http://localhost:8086/api/users"
  // Load users on page entry
        document.getElementById("save-button").addEventListener("click", function () {
            const practiceTime = document.getElementById("practice-time").value;
            const name = document.getElementById("name").value;
            const instrument = document.getElementById("instrument").value;
            if (practiceTime !== "" && name !== "" && instrument !== "") {
                const currentDate = new Date().toLocaleDateString();
                const practiceData = JSON.parse(localStorage.getItem(name)) || {};
                practiceData[currentDate] = {
                    instrument: instrument,
                    time: parseInt(practiceTime),
                };
                localStorage.setItem(name, JSON.stringify(practiceData));
                alert(`Practice time (${practiceTime} minutes) saved for ${currentDate}.`);
                if (parseInt(practiceTime) < 15) {
                    alert("Try to practice more tomorrow!");
                } else {
                    alert("Great job, keep it up!");
                }
                document.getElementById("practice-time").value = "";
                // Refresh the practice log display
                displayWeeklyLog();
            } else {
                alert("Please enter a valid practice time, name, and instrument.");
            }
        });
        // Function to display the weekly practice log
        function displayWeeklyLog() {
            fetch('https://bella-flask-portfolio.stu.nighthawkcodingsociety.com/api/users')
            .then(response => response.json())
            .then(data => {
                const responseElement = document.getElementById('api-response');
                const tableBody = document.querySelector("#weekly-log table tbody");
                //tableBody.innerHTML=JSON.stringify(data, null, 2);
                //responseElement.innerHTML = JSON.stringify(data, null, 2);
                 data.forEach(item => {
                    const row = tableBody.insertRow();
                    const idCell = row.insertCell(0);
                    const nameCell = row.insertCell(1);
                    const emailCell = row.insertCell(2);
                    const email2Cell = row.insertCell(3);
                    idCell.textContent = item.name;
                    nameCell.textContent = item.age;
;                   emailCell.textContent = item.tracking;
                    email2Cell.textContent = item.tracking.practiceTime;
                    const nestedArray = item.tracking;                                    
                });
            })
            .catch(error => {
                console.error('Error:', error);
            });
            const tableBody = document.querySelector("#weekly-log table tbody");
            tableBody.innerHTML = "";
            // Iterate through all local storage keys (which are user names)            
            for (let i = 0; i < localStorage.length; i++) {
                const name = localStorage.key(i);
                const practiceData = JSON.parse(localStorage.getItem(name)) || {};
                for (const date in practiceData) {
                    const row = tableBody.insertRow();
                    const cellName = row.insertCell(0);
                    const cellDate = row.insertCell(1);
                    const cellInstrument = row.insertCell(2);
                    const cellTime = row.insertCell(3);
                    cellName.textContent = name;
                    cellDate.textContent = date;
                    cellInstrument.textContent = practiceData[date].instrument;
                    cellTime.textContent = practiceData[date].time;
                }
            }            
        }
        // Call the function to display the weekly practice log when the page loads
         // prepare HTML result container for new output
        displayWeeklyLog();
        //read_users();
        

      

 </script>


    
</body>
</html>
