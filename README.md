<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>M Raza Call Later</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="app-container">
        <h1>M Raza Call Later</h1>

        <div class="input-group">
            <label for="phoneNumber">Enter Phone Number:</label>
            <input type="tel" id="phoneNumber" placeholder="Enter phone number" required>
        </div>

        <div class="input-group">
            <label for="callTime">Select Call Time:</label>
            <input type="datetime-local" id="callTime" required>
        </div>

        <button id="scheduleCallButton">Schedule Call</button>

        <div id="confirmationMessage"></div>
    </div>

    <script src="script.js"></script>
</body>
</html>

document.addEventListener('DOMContentLoaded', function() {
    const scheduleCallButton = document.getElementById('scheduleCallButton');
    const confirmationMessage = document.getElementById('confirmationMessage');

    scheduleCallButton.addEventListener('click', function() {
        const phoneNumber = document.getElementById('phoneNumber').value.trim();
        const callTime = document.getElementById('callTime').value;

        // Validate phone number and time input
        if (!validatePhoneNumber(phoneNumber)) {
            confirmationMessage.textContent = 'Please enter a valid phone number.';
            confirmationMessage.style.color = 'red';
            return;
        }

        if (!callTime) {
            confirmationMessage.textContent = 'Please select a valid call time.';
            confirmationMessage.style.color = 'red';
            return;
        }

        const scheduledTime = new Date(callTime).getTime();
        const currentTime = new Date().getTime();

        if (scheduledTime <= currentTime) {
            confirmationMessage.textContent = 'Please choose a future time.';
            confirmationMessage.style.color = 'red';
            return;
        }

        const delay = scheduledTime - currentTime;
        confirmationMessage.textContent = `Call scheduled for ${new Date(callTime).toLocaleString()}.`;
        confirmationMessage.style.color = 'green';

        // Schedule the call
        setTimeout(function() {
            initiateCall(phoneNumber);
        }, delay);
    });

    // Validate phone number format (basic check)
    function validatePhoneNumber(phoneNumber) {
        const phoneRegex = /^[0-9]{10,15}$/;
        return phoneRegex.test(phoneNumber);
    }

    // Function to initiate a call
    function initiateCall(phoneNumber) {
        const confirmation = confirm(`It's time to call ${phoneNumber}. Do you want to proceed?`);
        if (confirmation) {
            window.location.href = `tel:${phoneNumber}`;
        } else {
            confirmationMessage.textContent = 'Call cancelled.';
            confirmationMessage.style.color = 'red';
        }
    }
});

document.addEventListener('DOMContentLoaded', function() {
    const scheduleCallButton = document.getElementById('scheduleCallButton');
    const confirmationMessage = document.getElementById('confirmationMessage');

    // Button click event listener
    scheduleCallButton.addEventListener('click', function() {
        const phoneNumber = document.getElementById('phoneNumber').value;
        const callTime = document.getElementById('callTime').value;

        if (phoneNumber && callTime) {
            const scheduledTime = new Date(callTime).getTime();
            const currentTime = new Date().getTime();
            const delay = scheduledTime - currentTime;

            if (delay > 0) {
                confirmationMessage.textContent = `Call scheduled for ${new Date(callTime).toLocaleString()}.`;

                // Set timeout to trigger the call later
                setTimeout(function() {
                    initiateCall(phoneNumber);
                }, delay);
            } else {
                confirmationMessage.textContent = 'Please choose a future time.';
            }
        } else {
            confirmationMessage.textContent = 'Please fill in all fields.';
        }
    });

    // Function to initiate a call
    function initiateCall(phoneNumber) {
        const confirmation = confirm(`It's time to call ${phoneNumber}. Do you want to proceed?`);

        if (confirmation) {
            window.location.href = `tel:${phoneNumber}`;
        } else {
            confirmationMessage.textContent = 'Call cancelled.';
        }
    }
});
