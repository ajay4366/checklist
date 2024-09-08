<!DOCTYPE html>
<html>
<head>
    <title>Solaroot Checklist</title>
    <style>
        body {
            font-size: 18px; /* Increase the base text size */
        }

        .form-container {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            justify-content: center; /* Center the form horizontally */
        }

        .form-group {
            display: flex;
            flex-direction: column;
        }

        .form-group select {
            width: 150px;
        }

        .form-button {
            margin-top: 20px;
            display: flex;
            justify-content: center; /* Center the button */
        }

        h1, h2 {
            text-align: center; /* Center both titles */
            font-size: 32px; /* Increase title text size */
        }

        #checklist {
            margin-top: 20px;
            display: flex;
            flex-direction: column;
            align-items: center; /* Center the checklist container */
        }

        .checklist-item {
            display: flex; /* Use flexbox to keep checkbox and label in the same row */
            align-items: center; /* Align items vertically in the center */
            width: 100%; /* Full width to help with alignment */
            max-width: 400px; /* Set a maximum width for the container */
            padding: 10px;
            margin-bottom: 10px;
            border-radius: 5px;
            transition: background-color 0.3s ease;
        }

        .checklist-item label {
            margin-left: 10px; /* Add some spacing between checkbox and text */
            flex: 1; /* Make label take up the remaining space */
        }

        /* Background color for checked items */
        input[type="checkbox"]:checked + label {
            background-color: lightgreen;
        }

        /* Increased checkbox size */
        input[type="checkbox"] {
            width: 30px; /* Width of the checkbox */
            height: 30px; /* Height of the checkbox */
            display: inline-block;
            margin-right: 10px;
        }
    </style>
    <script>
        async function fetchDropdownData() {
            const response = await fetch('https://script.google.com/macros/s/AKfycbyKFUZB5tzvJGmnS3Wvm7-RTFc7MEh2pBut46iXUBvWyFchOZ8MSxBVeqYIJg8Svrmi/exec'); // Replace with your Web App URL
            const data = await response.json();
            return data;
        }

        function populateDropdown(id, options) {
            const dropdown = document.getElementById(id);
            dropdown.innerHTML = ''; // Clear existing options

            options.forEach(option => {
                const opt = document.createElement('option');
                opt.value = option;
                opt.innerHTML = option;
                dropdown.appendChild(opt);
            });
        }

        async function initializeDropdowns() {
            const data = await fetchDropdownData();

            populateDropdown('state', data.stateData);
            populateDropdown('client', data.clientData);
            populateDropdown('ahj', data.ahjData);
            populateDropdown('utility', data.utilityData);
            populateDropdown('projectType', data.projectTypeData);
        }

        async function generateChecklist() {
            var state = document.getElementById("state").value;
            var client = document.getElementById("client").value;
            var ahj = document.getElementById("ahj").value;
            var utility = document.getElementById("utility").value;
            var projectType = document.getElementById("projectType").value;

            var checklist = [
                "Project Kickoff - Initial client meeting",
                "Design Phase - Initial design draft",
                "Compliance Check - Code compliance",
                "Utility Coordination - Submit necessary documents",
                "Client Approval - Client feedback and revisions"
            ];

            if (state == "California") {
                checklist.push("Title 24 Compliance");
            } else if (state == "New York") {
                checklist.push("New York State Energy Code Compliance");
            }

            if (client == "Client A") {
                checklist.push("Sustainability Documentation");
            } else if (client == "Client B") {
                checklist.push("Weekly update reports for Client B");
            }

            if (ahj == "AHJ X") {
                checklist.push("Fire Code Inspection");
            } else if (ahj == "AHJ Y") {
                checklist.push("Environmental Review");
            }

            if (utility == "Utility A") {
                checklist.push("Renewable Energy Approval");
            } else if (utility == "Utility B") {
                checklist.push("Grid Connection Permit");
            }

            if (projectType == "Residential") {
                checklist.push("Homeowner Consultation");
            } else if (projectType == "Commercial") {
                checklist.push("Commercial Building Code Review");
            }

            var checklistHTML = "";
            checklist.forEach(function(item, index) {
                checklistHTML += '<div class="checklist-item">';
                checklistHTML += '<input type="checkbox" id="checklistItem' + index + '">';
                checklistHTML += '<label for="checklistItem' + index + '">' + (index + 1) + ". " + item + '</label>';
                checklistHTML += '</div>';
            });

            document.getElementById("checklist").innerHTML = checklistHTML;
        }

        // Initialize dropdowns on page load
        window.onload = initializeDropdowns;
    </script>
</head>
<body>
    <h1>Solaroot Checklist</h1> <!-- Updated Title -->

    <div class="form-container">
        <div class="form-group">
            <label for="state">Select State:</label>
            <select id="state"></select>
        </div>

        <div class="form-group">
            <label for="client">Select Client:</label>
            <select id="client"></select>
        </div>

        <div class="form-group">
            <label for="ahj">Select AHJ:</label>
            <select id="ahj"></select>
        </div>

        <div class="form-group">
            <label for="utility">Select Utility:</label>
            <select id="utility"></select>
        </div>

        <div class="form-group">
            <label for="projectType">Select Project Type:</label>
            <select id="projectType"></select>
        </div>
    </div>

    <div class="form-button">
        <button onclick="generateChecklist()">Generate Checklist</button>
    </div>

    <h2>Your Checklist:</h2>
    <div id="checklist"></div>
</body>
</html>
