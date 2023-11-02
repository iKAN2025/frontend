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
            padding: 50px;
            background-color: rgb(183, 255, 217);
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
        <button id="save-button">Create User</button>
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
         const url = "http://127.0.0.1:8240/api/users/"   
        // JavaScript to save practice time to local storage
            document.getElementById("save-button").addEventListener("click", async function () {
            //alert('Saving function');
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
                //alert(`Practice time (${practiceTime} minutes) saved for ${currentDate}.`);
                if (parseInt(practiceTime) < 15) {
                    alert("Try to practice more tomorrow!");
                } else {
                    alert("Great job, keep it up!");
                }
                 // Create a JSON object for tracking data
                    var tracking_string = { 
                     "userName": name, 
                     "instrumentName": instrument,
                     "practiceDate": currentDate,
                     "practiceTime": practiceTime
                    };
                    let randm=Math.floor(Math.random() * 100) + 1;
                    let txtuid = name+ randm.toString();
                    //alert(txtuid);
                var data2 = {  //the other data that needs to be sent
                     "id": "3",
                     "name": name,
                     "uid": txtuid,  
                     "dob": dob,
                     "age": "16",
                     "tracking": tracking_string //tracking string
                };
                //alert(dob);
                 // Convert the JSON object to a string
               var jsonData = JSON.stringify(data2); //make data2 a string 
  // Call the API with the JSON payload
                try { //try and catch
                   // alert ('Calling the createUser Function');
                     await postToAPI(jsonData);
                   // await createUser(name, dob, instrument, currentDate, practiceTime);
                }
                catch (e) {
                    console.log(e) //log error
                }
                displayWeeklyLog(); //run function
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
        // syntactic sugar approach!  is syntax within a programming language that is designed to make things easier to read or to express. It makes the language "sweeter" for human use: things can be expressed more clearly, more concisely, or in an alternative style that some may prefer.
        async function postToAPI(jsonData) {
            //alert(JSON.stringify(jsonData));
            //alert("Calling postToAPI");
            //alert(url);
            fetch(url, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: jsonData
            })
            .then(response => {
                if (response.ok) {
                    // Handle success
                    alert("Practice data has been added successfully");
                    console.log("Data sent successfully!");
                     document.getElementById("name").value = "";
                     document.getElementById("dob").value = "";
                     document.getElementById("practice-time").value = "";
                     document.getElementById("instrument").value = "";
                     displayWeeklyLog();
                } else {
                    // Handle errors
                    console.error("Failed to send data to the API.");
                    alert("Failed");
                     throw Error(`Error: ${response?.error?.data}`) ; //there's no guarantee respsonse data
                }
            })
           // .catch(error => {
                // Handle network errors
           //     console.error("Network error:", error);
           //     alert(error);
           // });
            //alert("end of postToAPI");
        }

 </script>

    
</body>
</html>