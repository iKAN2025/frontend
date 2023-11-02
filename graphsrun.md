<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tracker Data Visualization</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
    <div style="max-width: 600px; margin: 0 auto;">
        <canvas id="practiceChart"></canvas>
    </div>
</body>
<script>
    const apiUrl = 'http://127.0.0.1:8240/api/users/';
// Fetch user data from the API
fetch(apiUrl)
    .then(response => response.json())
    .then(data => {
        // Process your data and create datasets for the chart
        const users = data.map(user => user.name);
        const practiceTimes = data.map(user => user.tracking.practiceTime);
        // Create a chart using Chart.js
        const ctx = document.getElementById('practiceChart').getContext('2d');
        new Chart(ctx, {
            type: 'bar',
            data: {
                labels: users,
                datasets: [
                    {
                        label: 'Practice Time (minutes)',
                        data: practiceTimes,
                        backgroundColor: 'rgba(75, 192, 192, 0.2)', // Customize colors
                        borderColor: 'rgba(75, 192, 192, 1)', // Customize colors
                        borderWidth: 1,
                    }
                ]
            },
            options: {
                scales: {
                    y: {
                        beginAtZero: true,
                        title: {
                            display: true,
                            text: 'Minutes'
                        }
                    }
                }
            }
        });
    })
    .catch(error => {
        console.error('Error fetching data:', error);
    });
</script>

