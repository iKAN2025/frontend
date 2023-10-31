<!-- put your HTML code in this cell, Make sure to press the Run button to see your results below -->

<!-- put your HTML code in this cell, Make sure to press the Run button to see your results below -->
<!-- put your HTML code in this cell, Make sure to press the Run button to see your results below -->

{% include passionheader.html %}
<body>
    <div class="container">
        <h1>Name</h1>
        <input type="text" id="name" placeholder="Enter first name" required>
        <h1>Date of Birth </h1>
        <input type="text" id="dob" placeholder="MM/DD/YYYY" required>
        <h1>Instrument</h1>
        <input type="text" id="instrument" placeholder="Enter instrument name" required>
        <h1>Number of Minutes Practiced</h1>
        <input type="number" id="practice-time" placeholder="Enter practice time (minutes)" required>
        <br><br>
        <button id="save-button">Save</button>
        <!-- Weekly Practice Log Display -->
        <div id="weekly-log">
            <h2>Weekly Practice Log</h2>
            <table>
                <thead>
                    <tr>
                        <th>Name</th>
                        <th>Date of Birth </th>
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
    <a href="https://pianopower.org/16-benefits-of-playing-an-instrument/" target="_blank" style="color: purple;">Benefits of Practicing</a> |
    <a href="https://www.betterup.com/blog/15-ways-to-improve-your-focus-and-concentration-skills" target="_blank" style="color: purple;">How to Increase Concentration</a>
</div>

    <script>
        // JavaScript to save practice time to local storage
        document.getElementById("save-button").addEventListener("click", async function () {
            const practiceTime = document.getElementById("practice-time").value;
            const name = document.getElementById("name").value;
            const dob = document.getElementById("dob").value;
            const instrument = document.getElementById("instrument").value;
            const currentDate = new Date().toLocaleDateString();
            if (practiceTime !== "" && name !== "" && instrument !== "") {
                const practiceData = JSON.parse(localStorage.getItem(name)) || {};
                practiceData[currentDate] = {
                    dob: dob,
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
                try {
                    await createUser(name, dob, instrument, currentDate, practiceTime);
                }
                catch (e) {
                    console.log(e)
                }
                displayWeeklyLog();
            } else {
                alert("Please enter a valid practice time, name, and instrument.");
            }
        });
        // Function to display the weekly practice log
        //The fetch function is used to make an HTTP GET request to the URL 'https://bella-flask-portfolio.stu.nighthawkcodingsociety.com/api/users'. This is typically how you request data from a remote server using JavaScript.
        //The .then method is used to handle the response from the API request. It is a part of JavaScript's Promise-based architecture, which allows asynchronous operations to be handled in a more structured manner.
        //Inside the first .then block, the response from the API is converted to JSON format using response.json(). This is assuming that the response from the server is in JSON format.
        //The code then proceeds to update the content of the web page. It first retrieves a DOM element with the ID 'api-response' and a table body element inside a table with the ID 'weekly-log'. These elements are going to be used to display the data retrieved from the API.
 function displayWeeklyLog() {
            fetch('https://bella-flask-portfolio.stu.nighthawkcodingsociety.com/api/users') //get api call
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
                    const cellName = row.insertCell();
                    const cellDOB = row.insertCell(1);
                    const cellDate = row.insertCell(2);
                    const cellInstrument = row.insertCell(3);
                    const cellTime = row.insertCell(4);
                    cellName.textContent = name;
                    cellDOB.textContent = practiceData[date].dob;
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
        // syntactic sugar approach!  is syntax within a programming language that is designed to make things easier to read or to express. It makes the language "sweeter" for human use: things can be expressed more clearly, more concisely, or in an alternative style that some may prefer.
        async function createUser(name, dob,  instrumentName, practiceDate, practiceTime) { 
            const url = "https://bella-flask-portfolio.stu.nighthawkcodingsociety.com/api/users"
            const headers = {
                "Content-Type":"application/json",
            }
            const body = {
                name: name,
                dob: dob,
                instrumentName: instrumentName,
                practiceDate: practiceDate,
                practiceTime: practiceTime,
            }
            const requestInit = {
                method:"POST",
                headers,
                body,
            }
            const response = await fetch(url, requestInit)
            if (!response.ok) {
                throw Error(`Error: ${response?.error?.data}`)  //no guarantee that value will be given
            }
            const data = await response.json()
            return data;
        }

 </script>


    
</body>
</html>


