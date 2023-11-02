<!-- put your HTML code in this cell, Make sure to press the Run button to see your results below -->

<!-- put your HTML code in this cell, Make sure to press the Run button to see your results below -->
<!-- put your HTML code in this cell, Make sure to press the Run button to see your results below -->
{% include passionheader.html %}
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
        // JavaScript to save practice time to local storage
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
                // tableBody.innerHTML=JSON.stringify(data, null, 2);
                // responseElement.innerHTML = JSON.stringify(data, null, 2);
                 data.forEach(item => {
                    var tracking = JSON.parse(item.tracking);
                    console.log(tracking);
                    const row = tableBody.insertRow();
                    const idCell = row.insertCell(0);
                    const nameCell = row.insertCell(1);
                    const emailCell = row.insertCell(2);
                    const email2Cell = row.insertCell(3);
                    idCell.textContent = tracking.userName;
                    nameCell.textContent = tracking.practiceDate;
                    emailCell.textContent = tracking.instrumentName;
                    email2Cell.textContent = tracking.practiceTime;
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
        // Call the function to display the weekly practice log when the page loads
        displayWeeklyLog();

        

 </script>


    
</body>
</html>
