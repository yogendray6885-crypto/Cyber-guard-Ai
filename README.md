<!DOCTYPE html>
<html lang="en" data-theme="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>CyberGuard AI - Google Login</title>
    <style>
        /* All CSS is the same as the previous correct version */
        :root {
            --bg-main: #0a0a0a; --bg-card: #161616; --bg-input: #222;
            --text-main: #ffffff; --text-sec: #a0a0a0; --border: #333;
            --accent-blue: #2979ff; --accent-green: #00e676; --accent-red: #ff3d00;
        }
        [data-theme="light"] {
            --bg-main: #f0f2f5; --bg-card: #ffffff; --bg-input: #e4e6eb;
            --text-main: #1a1a1a; --text-sec: #606770; --border: #dddfe2;
        }
        * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; outline: none; }
        body {
            background-color: var(--bg-main); color: var(--text-main);
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            margin: 0; display: flex; flex-direction: column; min-height: 100vh; overflow-x: hidden;
            transition: background-color 0.2s, color 0.2s;
        }
        .hidden { display: none !important; }
        #app-loader {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: var(--bg-main); z-index: 10000;
            display: flex; align-items: center; justify-content: center;
        }
        .loader-spinner {
            width: 40px; height: 40px; border: 4px solid var(--border);
            border-top: 4px solid var(--accent-blue); border-radius: 50%;
            animation: spin 1s linear infinite;
        }
        @keyframes spin { 100% { transform: rotate(360deg); } }
        .btn {
            width: 100%; padding: 14px; border-radius: 10px; border: none;
            font-weight: 600; font-size: 1rem; cursor: pointer; margin-top: 15px;
            color: white; transition: transform 0.1s;
            display: flex; align-items: center; justify-content: center; gap: 10px;
        }
        .btn:active { transform: scale(0.97); }
        .btn-primary { background: var(--accent-blue); }
        /* NEW GOOGLE BUTTON STYLE */
        .btn-google {
            background: #ffffff;
            color: #444;
            border: 1px solid var(--border);
        }
        .input-box {
            width: 100%; padding: 14px; background: var(--bg-input);
            border: 1px solid var(--border); color: var(--text-main);
            border-radius: 8px; margin-bottom: 12px; font-size: 1rem;
        }
        .screen { padding: 25px; padding-bottom: 90px; animation: fadeIn 0.4s; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        .header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; }
        .theme-toggle { font-size: 1.5rem; background: none; border: none; cursor: pointer; padding: 5px; }
        .score-card {
            background: var(--bg-card); padding: 20px; border-radius: 20px;
            text-align: center; border: 1px solid var(--border);
            margin-bottom: 20px;
        }
        .score-ring {
            width: 130px; height: 130px; border-radius: 50%;
            border: 8px solid var(--bg-input); margin: 0 auto 15px;
            display: flex; align-items: center; justify-content: center;
            font-size: 2.2rem; font-weight: 700; color: var(--text-main);
        }
        .grid-menu { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; }
        .grid-item {
            background: var(--bg-card); padding: 18px; border-radius: 15px;
            text-align: center; border: 1px solid var(--border); cursor: pointer;
            font-weight: 500;
        }
        .modal-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.9); z-index: 9999;
            backdrop-filter: blur(8px); display: flex; align-items: center; justify-content: center;
        }
        .modal-content {
            background: var(--bg-card); padding: 25px; border-radius: 16px;
            width: 90%; max-width: 400px; text-align: center;
            border: 1px solid var(--border);
        }
        .ad-banner-container {
            background: var(--bg-input); border: 1px dashed var(--border);
            margin: 20px 0; min-height: 70px;
            display: flex; justify-content: center; align-items: center;
            overflow: hidden; border-radius: 8px;
        }
        .footer {
            position: fixed; bottom: 0; width: 100%; background: var(--bg-card);
            text-align: center; padding: 12px; font-size: 0.75rem; color: var(--text-sec);
            border-top: 1px solid var(--border); z-index: 100;
        }
        .divider {
            display: flex; align-items: center; text-align: center; color: var(--text-sec);
            margin: 20px 0;
        }
        .divider::before, .divider::after {
            content: ''; flex: 1; border-bottom: 1px solid var(--border);
        }
        .divider:not(:empty)::before { margin-right: .25em; }
        .divider:not(:empty)::after { margin-left: .25em; }
    </style>
</head>
<body>

    <div id="app-loader"><div class="loader-spinner"></div></div>
    <div id="modal-container"></div>

    <!-- Login Screen -->
    <div id="screen-login" class="screen hidden">
        <div style="text-align:center; margin: 60px 0 40px 0;">
            <div style="font-size:3.5rem;">🛡️</div>
            <h1 style="color: var(--accent-blue);">CYBERGUARD</h1>
            <p style="color: var(--text-sec);">Cloud Grade Security</p>
        </div>
        <div style="max-width:400px; margin: 0 auto;">
            <input type="email" id="email" class="input-box" placeholder="Email Address" autocomplete="email">
            <input type="password" id="password" class="input-box" placeholder="Password" autocomplete="current-password">
            <button class="btn btn-primary" onclick="Auth.signInWithEmail()">Login / Register</button>
            
            <div class="divider">OR</div>

            <!-- NEW GOOGLE LOGIN BUTTON -->
            <button class="btn btn-google" onclick="Auth.signInWithGoogle()">
                <svg xmlns="http://www.w3.org/2000/svg" x="0px" y="0px" width="20" height="20" viewBox="0 0 48 48">
                    <path fill="#FFC107" d="M43.611,20.083H42V20H24v8h11.303c-1.649,4.657-6.08,8-11.303,8c-6.627,0-12-5.373-12-12c0-6.627,5.373-12,12-12c3.059,0,5.842,1.154,7.961,3.039l5.657-5.657C34.046,6.053,29.268,4,24,4C12.955,4,4,12.955,4,24c0,11.045,8.955,20,20,20c11.045,0,20-8.955,20-20C44,22.659,43.862,21.35,43.611,20.083z"></path><path fill="#FF3D00" d="M6.306,14.691l6.571,4.819C14.655,15.108,18.961,12,24,12c3.059,0,5.842,1.154,7.961,3.039l5.657-5.657C34.046,6.053,29.268,4,24,4C16.318,4,9.656,8.337,6.306,14.691z"></path><path fill="#4CAF50" d="M24,44c5.166,0,9.86-1.977,13.409-5.192l-6.19-5.238C29.211,35.091,26.715,36,24,36c-5.202,0-9.619-3.317-11.283-7.946l-6.522,5.025C9.505,39.556,16.227,44,24,44z"></path><path fill="#1976D2" d="M43.611,20.083H42V20H24v8h11.303c-0.792,2.237-2.231,4.166-4.087,5.571l6.19,5.238C44.434,36.316,48,29.936,48,24c0-1.94-0.291-3.82-0.811-5.613L43.611,20.083z"></path>
                </svg>
                Sign in with Google
            </button>

            <div id="login-msg" style="text-align: center; margin-top: 20px; color: var(--accent-red); min-height:1em; font-weight: 500;"></div>
        </div>
    </div>

    <!-- Dashboard Screen -->
    <div id="screen-dashboard" class="screen hidden">
        <div class="header">
            <div><h2 style="margin:0;">Dashboard</h2><small id="user-display" style="color: var(--text-sec);">...</small></div>
            <button class="theme-toggle" onclick="Theme.toggle()" id="theme-btn">🌙</button>
        </div>
        <div class="score-card">
            <div class="score-ring" id="score-ring">0%</div>
            <button class="btn btn-primary" onclick="App.scan()">⚡ Smart System Scan</button>
        </div>
        <div class="ad-banner-container" id="dashboard-ad-container"></div>
        <div class="grid-menu">
            <div class="grid-item" onclick="Nav.to('Vault')">🔐 Vault</div>
            <div class="grid-item" onclick="Nav.to('AI Check')">🤖 AI Check</div>
            <div class="grid-item" onclick="Nav.to('Tools')">🛡️ Tools</div>
            <div class="grid-item" onclick="Nav.to('Settings')">⚙️ Settings</div>
        </div>
    </div>
    
    <div class="footer">CyberGuard AI | Stable Build</div>

    <script type="module">
        // Import all necessary functions, including Google Auth
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getAuth, signInWithEmailAndPassword, createUserWithEmailAndPassword, signOut, onAuthStateChanged, GoogleAuthProvider, signInWithPopup } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";

        const firebaseConfig = {
            apiKey: "AIzaSyAAJ6Rj7CrVy3jdfopOjGEUcnTPczjyJT8",
            authDomain: "gen-lang-client-0088239422.firebaseapp.com",
            databaseURL: "https://gen-lang-client-0088239422-default-rtdb.firebaseio.com",
            projectId: "gen-lang-client-0088239422",
            storageBucket: "gen-lang-client-0088239422.firebasestorage.app",
            messagingSenderId: "351972056292",
            appId: "1:351972056292:web:8a06a1718454e8a6bee0f1",
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        window.isUserAuthenticated = false;

        window.Theme = { /* Theme logic remains the same */
            toggle: () => {
                const current = document.documentElement.getAttribute('data-theme');
                const next = current === 'dark' ? 'light' : 'dark';
                document.documentElement.setAttribute('data-theme', next);
                localStorage.setItem('cg_theme', next);
                document.getElementById('theme-btn').innerText = next === 'dark' ? '🌙' : '☀️';
            },
            init: () => {
                const saved = localStorage.getItem('cg_theme') || 'dark';
                document.documentElement.setAttribute('data-theme', saved);
            }
        };

        window.AdsManager = { /* AdsManager logic remains the same */
            pendingAction: null, lastAdShownTime: 0, adCooldown: 60000, isInitialized: false,
            init: () => {
                if (window.AdsManager.isInitialized) return;
                history.pushState(null, null, location.href);
                window.onpopstate = () => {
                    history.pushState(null, null, location.href);
                    if (window.isUserAuthenticated) { window.AdsManager.showInterstitial(() => {}); }
                };
                window.AdsManager.isInitialized = true;
            },
            showInterstitial: (callback) => {
                const now = Date.now();
                if (now - window.AdsManager.lastAdShownTime < window.AdsManager.adCooldown) { callback(); return; }
                window.AdsManager.pendingAction = callback;
                window.AdsManager.lastAdShownTime = now;
                const modalHTML = `<div id="ad-modal" class="modal-overlay"><div class="modal-content"><h3>Security Verification</h3><p style="color: var(--text-sec); font-size: 0.8rem; margin-bottom: 20px;">Please wait...</p><div class="ad-banner-container" id="interstitial-ad-slot"></div><button class="btn btn-outline" style="border-color: var(--accent-blue); color: var(--accent-blue);" onclick="AdsManager.close()">❌ Close & Continue</button></div></div>`;
                document.getElementById('modal-container').innerHTML = modalHTML;
                window.AdsManager.placeBanner('interstitial-ad-slot');
            },
            close: () => {
                document.getElementById('modal-container').innerHTML = "";
                if (window.AdsManager.pendingAction) { window.AdsManager.pendingAction(); window.AdsManager.pendingAction = null; }
            },
            placeBanner: (containerId) => {
                const container = document.getElementById(containerId);
                if (!container) return;
                const adScript = `atOptions = {'key' : 'ac7411ad1c4de0e2a8b62b4fac4711d7', 'format' : 'iframe', 'height' : 60, 'width' : 468, 'params' : {}}; document.write('<scr' + 'ipt type="text/javascript" src="http' + (location.protocol === 'https:' ? 's' : '') + '://www.highperformanceformat.com/ac7411ad1c4de0e2a8b62b4fac4711d7/invoke.js"></scr' + 'ipt>');`;
                const scriptEl = document.createElement('script');
                scriptEl.innerHTML = adScript;
                container.innerHTML = ''; container.appendChild(scriptEl);
            }
        };

        window.Nav = { to: (screenName) => { window.AdsManager.showInterstitial(() => { alert(`Navigating to ${screenName}.`); }); } };
        
        // --- 🔐 AUTHENTICATION (UPDATED with Google Sign-In) ---
        window.Auth = {
            signInWithEmail: () => {
                const email = document.getElementById('email').value;
                const password = document.getElementById('password').value;
                const msg = document.getElementById('login-msg');

                if (!email || !password) { msg.innerText = "Please enter both email and password."; return; }
                msg.innerText = "Authenticating...";

                signInWithEmailAndPassword(auth, email, password)
                    .then(() => { msg.innerText = ""; })
                    .catch(error => {
                        switch (error.code) {
                            case 'auth/user-not-found':
                                msg.innerText = "User not found. Creating new account...";
                                createUserWithEmailAndPassword(auth, email, password)
                                    .then(() => { msg.innerText = ""; })
                                    .catch(createError => { msg.innerText = `Registration failed: ${createError.message}`; });
                                break;
                            case 'auth/wrong-password':
                            case 'auth/invalid-credential':
                                msg.innerText = "Incorrect password. Please try again.";
                                break;
                            default:
                                msg.innerText = `Error: ${error.message}`;
                        }
                    });
            },
            
            // NEW GOOGLE SIGN-IN FUNCTION
            signInWithGoogle: () => {
                const provider = new GoogleAuthProvider();
                const msg = document.getElementById('login-msg');
                msg.innerText = "Opening Google Sign-In...";

                signInWithPopup(auth, provider)
                    .then((result) => {
                        // Successful sign-in. onAuthStateChanged will handle the UI change.
                        msg.innerText = "";
                        console.log("Google Sign-In Successful for:", result.user.displayName);
                    })
                    .catch((error) => {
                        // Handle errors here.
                        console.error("Google Sign-In Error:", error);
                        msg.innerText = `Google Sign-In Failed: ${error.message}`;
                    });
            }
        };
        
        window.App = {
            scan: () => { /* scan logic remains the same */
                const ring = document.getElementById('score-ring');
                ring.innerText = "...";
                ring.style.borderColor = "var(--accent-blue)";
                setTimeout(() => { ring.innerText = "100%"; ring.style.borderColor = "var(--accent-green)"; alert("System Secure."); }, 1200);
            },
            initDashboard: (user) => { /* initDashboard logic remains the same */
                document.getElementById('user-display').innerText = user.displayName || user.email; // Use displayName if available from Google
                document.getElementById('theme-btn').innerText = (localStorage.getItem('cg_theme') || 'dark') === 'dark' ? '🌙' : '☀️';
                AdsManager.placeBanner('dashboard-ad-container');
            }
        };
        
        onAuthStateChanged(auth, (user) => {
            const loader = document.getElementById('app-loader');
            if (user) {
                window.isUserAuthenticated = true;
                document.getElementById('screen-login').classList.add('hidden');
                document.getElementById('screen-dashboard').classList.remove('hidden');
                App.initDashboard(user);
                AdsManager.init();
            } else {
                window.isUserAuthenticated = false;
                document.getElementById('screen-dashboard').classList.add('hidden');
                document.getElementById('screen-login').classList.remove('hidden');
            }
            setTimeout(() => loader.classList.add('hidden'), 500);
        });

        window.onload = () => { Theme.init(); };
    </script>
</body>
</html>