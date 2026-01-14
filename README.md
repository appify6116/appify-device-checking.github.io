<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Device Verification</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        :root {
            --bg-color: #0f1115;
            --card-bg: #1c1f26;
            --primary: #3b82f6;
            --success: #10b981;
            --text-main: #ffffff;
            --text-sub: #9ca3af;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif; }

        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
            overflow-x: hidden;
            padding-bottom: 70px; /* Space for sticky ad */
        }

        /* HEADER */
        .header {
            width: 100%;
            padding: 20px;
            text-align: center;
            background: linear-gradient(180deg, rgba(59,130,246,0.1) 0%, rgba(15,17,21,0) 100%);
        }

        .shield-icon {
            font-size: 40px;
            margin-bottom: 10px;
            animation: pulse 2s infinite;
        }

        /* DEVICE INFO CARD */
        .info-card {
            background-color: var(--card-bg);
            width: 90%;
            max-width: 400px;
            border-radius: 16px;
            padding: 20px;
            margin: 10px 0;
            box-shadow: 0 4px 20px rgba(0,0,0,0.3);
            border: 1px solid rgba(255,255,255,0.05);
        }

        .info-row {
            display: flex;
            justify-content: space-between;
            padding: 8px 0;
            border-bottom: 1px solid rgba(255,255,255,0.05);
            font-size: 14px;
        }
        .info-row:last-child { border-bottom: none; }
        .label { color: var(--text-sub); }
        .value { font-weight: 600; color: var(--primary); }

        /* STATUS & TIMER */
        .status-box {
            margin: 20px 0;
            text-align: center;
        }

        .status-text {
            font-size: 18px;
            font-weight: 700;
            margin-bottom: 5px;
        }

        .timer-circle {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            border: 4px solid var(--card-bg);
            border-top: 4px solid var(--primary);
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 10px auto;
            font-size: 20px;
            font-weight: bold;
            animation: spin 1s linear infinite;
        }
        
        .timer-text { animation: none !important; }

        /* AD CONTAINERS */
        .ad-container-main {
            margin: 20px 0;
            width: 300px;
            height: 250px;
            background-color: #000;
            display: flex;
            align-items: center;
            justify-content: center;
            overflow: hidden;
            border-radius: 8px;
        }

        .ad-sticky-footer {
            position: fixed;
            bottom: 0;
            left: 0;
            width: 100%;
            height: 60px;
            background-color: #000;
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            border-top: 1px solid #333;
        }

        /* ANIMATIONS */
        @keyframes pulse {
            0% { transform: scale(1); opacity: 1; }
            50% { transform: scale(1.1); opacity: 0.8; }
            100% { transform: scale(1); opacity: 1; }
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .hidden { display: none; }
        .success { color: var(--success); }
        
    </style>
</head>
<body>

    <!-- Header -->
    <div class="header">
        <div class="shield-icon">🛡️</div>
        <h2>Security Check</h2>
        <p style="color: var(--text-sub); font-size: 13px;">Analyzing connection...</p>
    </div>

    <!-- 1. MANDATORY MAIN AD (300x250) -->
    <div class="ad-container-main" id="mainAd">
        <!-- Exact Ad Code Provided -->
        <script type="text/javascript">
            atOptions = {
                'key' : 'a6b55d13bb436cb0566409a9e985582c',
                'format' : 'iframe',
                'height' : 250,
                'width' : 300,
                'params' : {}
            };
        </script>
        <script type="text/javascript" src="https://www.highperformanceformat.com/a6b55d13bb436cb0566409a9e985582c/invoke.js"></script>
    </div>

    <!-- Status & Timer -->
    <div class="status-box">
        <div id="statusText" class="status-text">Verification in Progress</div>
        <div id="timerSpinner" class="timer-circle">
            <span id="countdown" class="timer-text">30</span>
        </div>
        <p style="font-size: 12px; color: var(--text-sub); margin-top:10px;">
            Please wait while we verify your device integrity.
        </p>
    </div>

    <!-- Device Details -->
    <div class="info-card">
        <div class="info-row">
            <span class="label">Device OS</span>
            <span class="value" id="osVal">Detecting...</span>
        </div>
        <div class="info-row">
            <span class="label">IP Address</span>
            <span class="value" id="ipVal">Scanning...</span>
        </div>
        <div class="info-row">
            <span class="label">Browser</span>
            <span class="value" id="browserVal">Detecting...</span>
        </div>
        <div class="info-row">
            <span class="label">Status</span>
            <span class="value" id="statusVal" style="color: #fbbf24;">Checking</span>
        </div>
    </div>

    <!-- 2. STICKY FOOTER AD (320x50) -->
    <div class="ad-sticky-footer">
        <!-- Exact Ad Code Provided -->
        <script type="text/javascript">
            atOptions = {
                'key' : '2423f7936ee6a6ce1b4c251bebbe1a49',
                'format' : 'iframe',
                'height' : 50,
                'width' : 320,
                'params' : {}
            };
        </script>
        <script type="text/javascript" src="https://www.highperformanceformat.com/2423f7936ee6a6ce1b4c251bebbe1a49/invoke.js"></script>
    </div>

    <script>
        // ================= CONFIGURATION =================
        const TARGET_CHANNEL_LINK = "https://t.me/appify0"; // Change this
        const VERIFICATION_TIME = 30; // Seconds
        // =================================================

        const tg = window.Telegram.WebApp;
        tg.expand(); // Make web app full screen

        // 1. Device Detection Logic
        function getOS() {
            let userAgent = window.navigator.userAgent,
                platform = window.navigator.platform,
                macosPlatforms = ['Macintosh', 'MacIntel', 'MacPPC', 'Mac68K'],
                windowsPlatforms = ['Win32', 'Win64', 'Windows', 'WinCE'],
                iosPlatforms = ['iPhone', 'iPad', 'iPod'],
                os = null;

            if (macosPlatforms.indexOf(platform) !== -1) os = 'Mac OS';
            else if (iosPlatforms.indexOf(platform) !== -1) os = 'iOS';
            else if (windowsPlatforms.indexOf(platform) !== -1) os = 'Windows';
            else if (/Android/.test(userAgent)) os = 'Android';
            else if (!os && /Linux/.test(platform)) os = 'Linux';

            return os || "Unknown Device";
        }

        // 2. IP Detection (Using free API)
        fetch('https://api.ipify.org?format=json')
            .then(response => response.json())
            .then(data => {
                document.getElementById('ipVal').innerText = data.ip;
            })
            .catch(error => {
                document.getElementById('ipVal').innerText = "Hidden";
            });

        // Set Static Info
        document.getElementById('osVal').innerText = getOS();
        document.getElementById('browserVal').innerText = "Telegram Webview";

        // 3. Countdown Logic
        let timeLeft = VERIFICATION_TIME;
        const countdownEl = document.getElementById('countdown');
        const statusText = document.getElementById('statusText');
        const statusVal = document.getElementById('statusVal');
        const spinner = document.getElementById('timerSpinner');

        const timer = setInterval(() => {
            timeLeft--;
            countdownEl.innerText = timeLeft;

            if (timeLeft <= 0) {
                clearInterval(timer);
                completeVerification();
            }
        }, 1000);

        // 4. Completion & Redirect Logic
        function completeVerification() {
            // UI Update
            spinner.style.border = "4px solid var(--success)";
            spinner.style.animation = "none";
            countdownEl.innerText = "✓";
            
            statusText.innerText = "Verified Successfully!";
            statusText.classList.add("success");
            
            statusVal.innerText = "Verified";
            statusVal.style.color = "var(--success)";

            // Optional: Hide Ad to clean up UI before redirect
            // document.getElementById('mainAd').style.display = 'none';

            // Redirect
            setTimeout(() => {
                // Using Telegram WebApp openLink for better compatibility
                tg.openTelegramLink(TARGET_CHANNEL_LINK);
                // Fallback
                window.location.href = TARGET_CHANNEL_LINK;
            }, 1500);
        }
    </script>
</body>
</html>
