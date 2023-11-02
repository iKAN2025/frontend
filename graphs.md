---
layout: default
title: Student Blog
permalink: /graphs
---

## Generated Graph For Each User







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

<style>
    .practice-heading {
        font-family: cursive;
        color: purple; 
    }
</style>

<style>
    .practice-heading {
        font-family: 'Arial', sans-serif;
        font-weight: bold;
        font-size: 24px; 
        color: purple
    }
</style>


<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tracker Data Visualization</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
    <div style="max-width: 800px; margin: 0 auto;">
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
                        backgroundColor: 'rgba(255, 105, 180, 0.2)', // Customize colors
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


<br>
<br>
<br>
<br>
<br>
<br>

<div class="practice-heading">
    Eun's Practice
    <iframe src="/frontend/passionproject/eun's_graph.html" width="600" height="350"></iframe>
</div>

<div class="practice-heading">
    Bella's Practice
    <iframe src="/frontend/passionproject/bella's_graph.html" width="600" height="350"></iframe>
</div>

<div class="practice-heading">
    Lakshanya's Practice
    <iframe src="/frontend/passionproject/lakshanya's_graph.html" width="600" height="350"></iframe>
</div>

<div class="practice-heading">
    Avanthika's Practice
    <iframe src="/frontend/passionproject/avanthika's_graph.html" width="600" height="350"></iframe>
</div>
