<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Complaint Management System</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --primary-color: #10b981;
            --primary-hover: #059669;
            --secondary-color: #1e293b;
            --accent-color: #f43f5e;
            --success-color: #10b981;
            --warning-color: #f59e0b;
            --info-color: #0ea5e9;
            --background-color: #f8fafc;
            --card-background: #ffffff;
            --text-primary: #1e293b;
            --text-secondary: #64748b;
            --text-light: #94a3b8;
            --border-color: #e2e8f0;
            --sidebar-width: 260px;
            --header-height: 70px;
            --border-radius: 10px;
            --box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
            --transition-speed: 0.3s;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
        }

        body {
            background-color: var(--background-color);
            color: var(--text-primary);
            display: flex;
            min-height: 100vh;
            overflow-x: hidden;
            width: 100%;
        }

        /* Sidebar Styles */
        .sidebar {
            width: var(--sidebar-width);
            height: 100vh;
            background: linear-gradient(135deg, var(--secondary-color), #0f172a);
            position: fixed;
            padding: 1.5rem 0;
            color: white;
            box-shadow: var(--box-shadow);
            z-index: 100;
            transition: transform var(--transition-speed);
        }

        .logo-container {
            padding: 1rem 2rem;
            margin-bottom: 2rem;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .logo {
            font-size: 1.5rem;
            font-weight: bold;
            display: flex;
            align-items: center;
            gap: 0.75rem;
            color: white;
        }

        .nav-links {
            display: flex;
            flex-direction: column;
            gap: 0.25rem;
            padding: 0 1rem;
        }

        .nav-link {
            padding: 0.875rem 1.25rem;
            color: rgba(255, 255, 255, 0.8);
            text-decoration: none;
            display: flex;
            align-items: center;
            gap: 0.75rem;
            border-radius: var(--border-radius);
            transition: all var(--transition-speed);
            font-weight: 500;
        }

        .nav-link:hover {
            background: rgba(255, 255, 255, 0.1);
            color: white;
            transform: translateX(4px);
        }

        .nav-link.active {
            background: var(--primary-color);
            color: white;
            font-weight: 600;
        }

        .nav-link i {
            width: 20px;
            text-align: center;
        }

        /* Main Content Styles */
        .main-content {
            margin-left: var(--sidebar-width);
            padding: 1.5rem;
            width: calc(100% - var(--sidebar-width));
            min-height: 100vh;
            transition: margin-left var(--transition-speed);
        }

        .card {
            background: var(--card-background);
            border-radius: var(--border-radius);
            box-shadow: var(--box-shadow);
            padding: 1.5rem;
            margin-bottom: 1.5rem;
            border: 1px solid var(--border-color);
            transition: transform 0.2s, box-shadow 0.2s;
            width: 100%; /* Ensure cards take full width */
        }

        .card:hover {
            box-shadow: 0 8px 24px rgba(0, 0, 0, 0.08);
        }

        h1 {
            margin-bottom: 1.5rem;
            color: var(--text-primary);
            font-weight: 700;
            font-size: 1.75rem;
            display: flex;
            align-items: center;
            gap: 0.75rem;
        }

        h2 {
            margin-bottom: 1.25rem;
            color: var(--text-primary);
            font-weight: 600;
            font-size: 1.25rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        /* Form Styles */
        .form-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 1.25rem;
            width: 100%; /* Ensure form grid takes full width */
        }

        .form-group {
            margin-bottom: 1.25rem;
            width: 100%; /* Ensure form groups take full width */
        }

        .form-label {
            display: block;
            margin-bottom: 0.5rem;
            color: var(--text-secondary);
            font-weight: 500;
            font-size: 0.9rem;
        }

        .form-control {
            width: 100%;
            padding: 0.75rem 1rem;
            border: 2px solid var(--border-color);
            border-radius: var(--border-radius);
            font-size: 0.95rem;
            transition: border-color 0.2s, box-shadow 0.2s;
            background-color: #fff;
        }

        .form-control:focus {
            outline: none;
            border-color: var(--primary-color);
            box-shadow: 0 0 0 3px rgba(16, 185, 129, 0.1);
        }

        select.form-control {
            appearance: none;
            background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='16' height='16' viewBox='0 0 24 24' fill='none' stroke='%2364748b' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3E%3Cpath d='M6 9l6 6 6-6'/%3E%3C/svg%3E");
            background-repeat: no-repeat;
            background-position: right 1rem center;
            padding-right: 2.5rem;
        }

        textarea.form-control {
            min-height: 120px;
            resize: vertical;
        }

        .btn {
            background: var(--primary-color);
            color: white;
            padding: 0.75rem 1.25rem;
            border: none;
            border-radius: var(--border-radius);
            font-size: 0.95rem;
            font-weight: 500;
            cursor: pointer;
            transition: background-color 0.2s, transform 0.1s;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 0.5rem;
        }

        .btn:hover {
            background: var(--primary-hover);
            transform: translateY(-1px);
        }

        .btn:active {
            transform: translateY(0);
        }

        /* Case ID and Status Styles */
        .case-id {
            background: #f1f5f9;
            padding: 0.5rem 1rem;
            border-radius: var(--border-radius);
            font-family: monospace;
            font-size: 1rem;
            color: var(--text-primary);
            margin-bottom: 1rem;
            display: inline-block;
            font-weight: 600;
        }

        .complaint-status {
            display: inline-flex;
            align-items: center;
            padding: 0.25rem 0.75rem;
            border-radius: 1rem;
            font-size: 0.875rem;
            font-weight: 500;
            margin-left: 1rem;
        }

        .status-pending {
            background: #fef3c7;
            color: #92400e;
        }

        .status-processing {
            background: #dbeafe;
            color: #1e40af;
        }

        .status-resolved {
            background: #dcfce7;
            color: #166534;
        }

        .complaint-actions {
            margin-top: 1.25rem;
            display: flex;
            gap: 0.75rem;
            flex-wrap: wrap;
        }

        .status-btn {
            padding: 0.5rem 1rem;
            border: none;
            border-radius: var(--border-radius);
            cursor: pointer;
            font-size: 0.875rem;
            font-weight: 500;
            transition: all 0.2s;
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;
        }

        .status-btn-processing {
            background: var(--info-color);
            color: white;
        }

        .status-btn-processing:hover {
            background: #0284c7;
        }

        .status-btn-resolved {
            background: var(--success-color);
            color: white;
        }

        .status-btn-resolved:hover {
            background: #059669;
        }

        /* Submissions List Styles */
        .submissions-list {
            margin-top: 1.5rem;
            width: 100%; /* Ensure submissions list takes full width */
        }

        .submission-item {
            background: #f8fafc;
            border: 1px solid var(--border-color);
            border-radius: var(--border-radius);
            padding: 1.25rem;
            margin-bottom: 1rem;
            transition: transform 0.2s, box-shadow 0.2s;
            width: 100%; /* Ensure submission items take full width */
        }

        .submission-item:hover {
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
            transform: translateY(-2px);
        }

        .submission-item p {
            margin: 0.5rem 0;
            color: var(--text-secondary);
        }

        .submission-item strong {
            color: var(--text-primary);
            font-weight: 600;
        }

        .delete-btn {
            background: var(--accent-color);
            color: white;
            padding: 0.5rem 1rem;
            border: none;
            border-radius: var(--border-radius);
            cursor: pointer;
            font-size: 0.875rem;
            font-weight: 500;
            transition: all 0.2s;
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;
        }

        .delete-btn:hover {
            background: #e11d48;
        }

        /* Dashboard Styles */
        .dashboard-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            gap: 1.25rem;
            margin-bottom: 1.5rem;
            width: 100%; /* Ensure dashboard grid takes full width */
        }

        .stat-card {
            background: var(--card-background);
            border-radius: var(--border-radius);
            box-shadow: var(--box-shadow);
            padding: 1.5rem;
            display: flex;
            flex-direction: column;
            align-items: center;
            text-align: center;
            border: 1px solid var(--border-color);
            transition: transform 0.2s, box-shadow 0.2s;
            height: 100%; /* Ensure all stat cards have the same height */
        }

        .stat-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 24px rgba(0, 0, 0, 0.08);
        }

        .stat-icon {
            width: 50px;
            height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
            margin-bottom: 1rem;
        }

        .icon-total {
            background: rgba(16, 185, 129, 0.1);
            color: var(--primary-color);
        }

        .icon-resolved {
            background: rgba(16, 185, 129, 0.1);
            color: var(--success-color);
        }

        .icon-processing {
            background: rgba(14, 165, 233, 0.1);
            color: var(--info-color);
        }

        .icon-pending {
            background: rgba(245, 158, 11, 0.1);
            color: var(--warning-color);
        }

        .stat-number {
            font-size: 2.25rem;
            font-weight: 700;
            margin: 0.5rem 0;
            color: var(--text-primary);
        }

        .stat-label {
            color: var(--text-secondary);
            font-size: 0.95rem;
            font-weight: 500;
        }

        .chart-container {
            height: 300px;
            position: relative;
            width: 100%; /* Ensure chart containers take full width */
        }

        .chart-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
            gap: 1.25rem;
            width: 100%; /* Ensure chart grid takes full width */
        }

        /* Auth Styles */
        .auth-container {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, var(--primary-color), var(--secondary-color));
            padding: 2rem;
            width: 100%;
        }

        .auth-card {
            background: white;
            border-radius: var(--border-radius);
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 450px;
            padding: 2.5rem;
            animation: fadeIn 0.5s ease-out;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(-20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .auth-header {
            text-align: center;
            margin-bottom: 2.5rem;
        }

        .auth-logo {
            font-size: 2rem;
            font-weight: bold;
            color: var(--primary-color);
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 0.75rem;
            margin-bottom: 1.5rem;
        }

        .auth-title {
            font-size: 1.75rem;
            color: var(--text-primary);
            margin-bottom: 0.75rem;
            font-weight: 700;
        }

        .auth-subtitle {
            color: var(--text-secondary);
            font-size: 1rem;
        }

        .auth-form {
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
            width: 100%; /* Ensure auth form takes full width */
        }

        .auth-input-group {
            position: relative;
            width: 100%; /* Ensure input groups take full width */
        }

        .auth-input {
            width: 100%;
            padding: 1rem 1.25rem;
            border: 2px solid var(--border-color);
            border-radius: var(--border-radius);
            font-size: 1rem;
            transition: border-color 0.2s, box-shadow 0.2s;
        }

        .auth-input:focus {
            border-color: var(--primary-color);
            outline: none;
            box-shadow: 0 0 0 3px rgba(16, 185, 129, 0.1);
        }

        .auth-btn {
            background: var(--primary-color);
            color: white;
            padding: 1rem;
            border: none;
            border-radius: var(--border-radius);
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: background-color 0.2s, transform 0.1s;
            margin-top: 0.5rem;
            width: 100%; /* Ensure auth button takes full width */
        }

        .auth-btn:hover {
            background: var(--primary-hover);
            transform: translateY(-1px);
        }

        .auth-btn:active {
            transform: translateY(0);
        }

        .auth-link {
            text-align: center;
            margin-top: 1.75rem;
            color: var(--text-secondary);
            font-size: 0.95rem;
        }

        .auth-link a {
            color: var(--primary-color);
            text-decoration: none;
            font-weight: 600;
            transition: color 0.2s;
        }

        .auth-link a:hover {
            color: var(--primary-hover);
            text-decoration: underline;
        }

        .auth-separator {
            display: flex;
            align-items: center;
            margin: 1.5rem 0;
        }

        .auth-separator-line {
            flex-grow: 1;
            height: 1px;
            background-color: var(--border-color);
        }

        .auth-separator-text {
            padding: 0 1rem;
            color: var(--text-secondary);
            font-size: 0.9rem;
        }

        .skip-btn {
            background: transparent;
            color: var(--text-secondary);
            border: 2px solid var(--border-color);
            padding: 1rem;
            border-radius: var(--border-radius);
            font-size: 1rem;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s;
            margin-top: 1rem;
            width: 100%; /* Ensure skip button takes full width */
        }

        .skip-btn:hover {
            background: #f8fafc;
            border-color: #cbd5e1;
            color: var(--text-primary);
        }

        .error-message {
            color: var(--accent-color);
            font-size: 0.875rem;
            margin-top: 0.5rem;
            font-weight: 500;
        }

        /* Responsive Styles */
        @media (max-width: 992px) {
            .chart-grid {
                grid-template-columns: 1fr;
            }
        }

        @media (max-width: 768px) {
            .sidebar {
                transform: translateX(-100%);
                width: 240px;
            }
            
            .sidebar.active {
                transform: translateX(0);
            }
            
            .main-content {
                margin-left: 0;
                width: 100%;
                padding: 1rem;
            }
            
            .mobile-header {
                display: flex;
                align-items: center;
                justify-content: space-between;
                padding: 1rem;
                background: white;
                box-shadow: var(--box-shadow);
                position: sticky;
                top: 0;
                z-index: 90;
                margin-bottom: 1rem;
                border-radius: var(--border-radius);
                width: 100%; /* Ensure mobile header takes full width */
            }
            
            .menu-toggle {
                background: none;
                border: none;
                color: var(--text-primary);
                font-size: 1.25rem;
                cursor: pointer;
                width: 40px;
                height: 40px;
                display: flex;
                align-items: center;
                justify-content: center;
                border-radius: 50%;
                transition: background-color 0.2s;
            }
            
            .menu-toggle:hover {
                background-color: #f1f5f9;
            }
            
            .mobile-logo {
                font-size: 1.25rem;
                font-weight: bold;
                color: var(--primary-color);
                display: flex;
                align-items: center;
                gap: 0.5rem;
            }
            
            .form-grid {
                grid-template-columns: 1fr;
            }
        }

        @media (min-width: 769px) {
            .mobile-header {
                display: none;
            }
        }

        /* Utility Classes */
        .mt-4 {
            margin-top: 1rem;
        }

        .mb-4 {
            margin-bottom: 1rem;
        }

        .text-center {
            text-align: center;
        }

        /* Animation */
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }

        .pulse {
            animation: pulse 2s infinite;
        }
        
        /* Overlay for mobile sidebar */
        .sidebar-overlay {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 99;
        }
        
        .sidebar-overlay.active {
            display: block;
        }
        
        /* Toast notification */
        .toast {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 1rem;
            border-radius: var(--border-radius);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            z-index: 1000;
            display: flex;
            align-items: center;
            gap: 0.75rem;
            max-width: 350px;
            animation: slideIn 0.3s ease-out forwards;
        }
        
        .toast-success {
            background: #dcfce7;
            color: #166534;
            border-left: 4px solid #10b981;
        }
        
        .toast-error {
            background: #fee2e2;
            color: #b91c1c;
            border-left: 4px solid #ef4444;
        }
        
        .toast-info {
            background: #dbeafe;
            color: #1e40af;
            border-left: 4px solid #3b82f6;
        }
        
        @keyframes slideIn {
            from { transform: translateX(100%); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }
        
        @keyframes slideOut {
            from { transform: translateX(0); opacity: 1; }
            to { transform: translateX(100%); opacity: 0; }
        }

        /* Fix for the auth container to center properly */
        #app {
            width: 100%;
            display: flex;
            flex-direction: column;
            min-height: 100vh;
        }

        /* Ensure equal height for stat cards */
        .dashboard-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            gap: 1.25rem;
            margin-bottom: 1.5rem;
        }

        .dashboard-grid > * {
            height: 100%;
        }

        /* Fix for chart containers to maintain proper aspect ratio */
        .chart-grid > .card {
            display: flex;
            flex-direction: column;
        }

        .chart-container {
            flex: 1;
            min-height: 300px;
        }
    </style>
</head>
<body>
    <div id="app">
        <!-- Content will be dynamically loaded here -->
    </div>
    
    <div class="sidebar-overlay" id="sidebarOverlay" onclick="toggleSidebar()"></div>

<script>
// Page Templates
const pageTemplates = {
    login: `
        <div class="auth-container">
            <div class="auth-card">
                <div class="auth-header">
                    <div class="auth-logo">
                        <i class="fas fa-clipboard-list"></i>
                        <span>ComplaintHub</span>
                    </div>
                    <h1 class="auth-title">Welcome Back</h1>
                    <p class="auth-subtitle">Sign in to your account to continue</p>
                </div>
                <form class="auth-form" id="loginForm">
                    <div class="auth-input-group">
                        <input type="email" class="auth-input" placeholder="Email Address" id="loginEmail" required>
                        <div class="error-message" id="loginEmailError"></div>
                    </div>
                    <div class="auth-input-group">
                        <input type="password" class="auth-input" placeholder="Password" id="loginPassword" required>
                        <div class="error-message" id="loginPasswordError"></div>
                    </div>
                    <button type="submit" class="auth-btn">
                        <i class="fas fa-sign-in-alt"></i> Sign In
                    </button>
                    <button type="button" class="skip-btn" onclick="skipAuth()">
                        <i class="fas fa-user-clock"></i> Continue as Guest
                    </button>
                </form>
                <div class="auth-link">
                    Don't have an account? <a href="#" onclick="loadPage('signup')">Create Account</a>
                </div>
            </div>
        </div>
    `,
    signup: `
        <div class="auth-container">
            <div class="auth-card">
                <div class="auth-header">
                    <div class="auth-logo">
                        <i class="fas fa-clipboard-list"></i>
                        <span>ComplaintHub</span>
                    </div>
                    <h1 class="auth-title">Create Account</h1>
                    <p class="auth-subtitle">Join us to start managing complaints</p>
                </div>
                <form class="auth-form" id="signupForm">
                    <div class="auth-input-group">
                        <input type="text" class="auth-input" placeholder="Full Name" id="signupName" required>
                        <div class="error-message" id="signupNameError"></div>
                    </div>
                    <div class="auth-input-group">
                        <input type="email" class="auth-input" placeholder="Email Address" id="signupEmail" required>
                        <div class="error-message" id="signupEmailError"></div>
                    </div>
                    <div class="auth-input-group">
                        <input type="password" class="auth-input" placeholder="Password" id="signupPassword" required>
                        <div class="error-message" id="signupPasswordError"></div>
                    </div>
                    <div class="auth-input-group">
                        <input type="password" class="auth-input" placeholder="Confirm Password" id="signupConfirmPassword" required>
                        <div class="error-message" id="signupConfirmPasswordError"></div>
                    </div>
                    <button type="submit" class="auth-btn">
                        <i class="fas fa-user-plus"></i> Create Account
                    </button>
                    <button type="button" class="skip-btn" onclick="skipAuth()">
                        <i class="fas fa-user-clock"></i> Continue as Guest
                    </button>
                </form>
                <div class="auth-link">
                    Already have an account? <a href="#" onclick="loadPage('login')">Sign In</a>
                </div>
            </div>
        </div>
    `,
    dashboard: `
        <div class="mobile-header">
            <button class="menu-toggle" onclick="toggleSidebar()">
                <i class="fas fa-bars"></i>
            </button>
            <div class="mobile-logo">
                <i class="fas fa-clipboard-list"></i>
                <span>ComplaintHub</span>
            </div>
            <button class="menu-toggle" onclick="logout()">
                <i class="fas fa-sign-out-alt"></i>
            </button>
        </div>
        <h1><i class="fas fa-chart-line"></i> Dashboard Overview</h1>
        <div class="dashboard-grid">
            <div class="stat-card">
                <div class="stat-icon icon-total">
                    <i class="fas fa-clipboard-list fa-lg"></i>
                </div>
                <div class="stat-number" id="totalComplaints">0</div>
                <div class="stat-label">Total Complaints</div>
            </div>
            <div class="stat-card">
                <div class="stat-icon icon-resolved">
                    <i class="fas fa-check-circle fa-lg"></i>
                </div>
                <div class="stat-number" id="resolvedComplaints">0</div>
                <div class="stat-label">Resolved</div>
            </div>
            <div class="stat-card">
                <div class="stat-icon icon-processing">
                    <i class="fas fa-spinner fa-lg"></i>
                </div>
                <div class="stat-number" id="processingComplaints">0</div>
                <div class="stat-label">Processing</div>
            </div>
            <div class="stat-card">
                <div class="stat-icon icon-pending">
                    <i class="fas fa-clock fa-lg"></i>
                </div>
                <div class="stat-number" id="pendingComplaints">0</div>
                <div class="stat-label">Pending</div>
            </div>
        </div>
        
        <div class="chart-grid">
            <div class="card">
                <h2><i class="fas fa-chart-pie"></i> Complaint Status Distribution</h2>
                <div class="chart-container">
                    <canvas id="statusChart"></canvas>
                </div>
            </div>
            <div class="card">
                <h2><i class="fas fa-chart-bar"></i> Service Type Distribution</h2>
                <div class="chart-container">
                    <canvas id="serviceChart"></canvas>
                </div>
            </div>
        </div>
        
        <div class="card">
            <h2><i class="fas fa-history"></i> Recent Complaints</h2>
            <div id="recentComplaints"></div>
        </div>
    `,
    citizenEngagement: `
        <div class="mobile-header">
            <button class="menu-toggle" onclick="toggleSidebar()">
                <i class="fas fa-bars"></i>
            </button>
            <div class="mobile-logo">
                <i class="fas fa-clipboard-list"></i>
                <span>ComplaintHub</span>
            </div>
            <button class="menu-toggle" onclick="logout()">
                <i class="fas fa-sign-out-alt"></i>
            </button>
        </div>
        <h1><i class="fas fa-users"></i> Citizen Engagement</h1>
        <div class="card">
            <h2><i class="fas fa-comment-alt"></i> New Inquiry</h2>
            <form onsubmit="handleSubmit(event, 'citizenEngagement')">
                <div class="form-grid">
                    <div class="form-group">
                        <label class="form-label">Citizen Name</label>
                        <input type="text" class="form-control" name="citizenName" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Email</label>
                        <input type="email" class="form-control" name="email" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Description</label>
                        <textarea class="form-control" name="description" rows="4" required></textarea>
                    </div>
                </div>
                <button type="submit" class="btn">
                    <i class="fas fa-paper-plane"></i> Submit Inquiry
                </button>
            </form>
        </div>
        <div class="submissions-list" id="citizenEngagementSubmissions"></div>
    `,
    complaintResolution: `
        <div class="mobile-header">
            <button class="menu-toggle" onclick="toggleSidebar()">
                <i class="fas fa-bars"></i>
            </button>
            <div class="mobile-logo">
                <i class="fas fa-clipboard-list"></i>
                <span>ComplaintHub</span>
            </div>
            <button class="menu-toggle" onclick="logout()">
                <i class="fas fa-sign-out-alt"></i>
            </button>
        </div>
        <h1><i class="fas fa-exclamation-circle"></i> Complaint Resolution</h1>
        <div class="card">
            <h2><i class="fas fa-file-alt"></i> File New Complaint</h2>
            <form onsubmit="handleComplaintSubmit(event)">
                <div class="form-grid">
                    <div class="form-group">
                        <label class="form-label">Full Name</label>
                        <input type="text" class="form-control" name="name" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Contact Number</label>
                        <input type="tel" class="form-control" name="contactNumber" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Email</label>
                        <input type="email" class="form-control" name="email" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Service Type</label>
                        <select class="form-control" name="serviceType" required>
                            <option value="">Select Service</option>
                            <option value="bus">Bus Service</option>
                            <option value="metro">Metro</option>
                            <option value="road">Road Maintenance</option>
                            <option value="water">Water Supply</option>
                            <option value="electricity">Electricity</option>
                            <option value="other">Other</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Complaint Details</label>
                        <textarea class="form-control" name="details" rows="4" required></textarea>
                    </div>
                </div>
                <button type="submit" class="btn">
                    <i class="fas fa-paper-plane"></i> Submit Complaint
                </button>
            </form>
        </div>
        <div class="submissions-list" id="complaintResolutionSubmissions"></div>
    `,
    permitProcessing: `
        <div class="mobile-header">
            <button class="menu-toggle" onclick="toggleSidebar()">
                <i class="fas fa-bars"></i>
            </button>
            <div class="mobile-logo">
                <i class="fas fa-clipboard-list"></i>
                <span>ComplaintHub</span>
            </div>
            <button class="menu-toggle" onclick="logout()">
                <i class="fas fa-sign-out-alt"></i>
            </button>
        </div>
        <h1><i class="fas fa-file-alt"></i> Permit Processing</h1>
        <div class="card">
            <h2><i class="fas fa-plus-circle"></i> New Permit Request</h2>
            <form onsubmit="handleSubmit(event, 'permitProcessing')">
                <div class="form-grid">
                    <div class="form-group">
                        <label class="form-label">Permit Type</label>
                        <select class="form-control" name="permitType" required>
                            <option value="">Select Type</option>
                            <option value="transportation">Transportation</option>
                            <option value="parking">Parking</option>
                            <option value="construction">Construction</option>
                            <option value="event">Event</option>
                            <option value="business">Business</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Applicant Name</label>
                        <input type="text" class="form-control" name="applicantName" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Contact Email</label>
                        <input type="email" class="form-control" name="email" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Request Details</label>
                        <textarea class="form-control" name="details" rows="4" required></textarea>
                    </div>
                </div>
                <button type="submit" class="btn">
                    <i class="fas fa-paper-plane"></i> Submit Request
                </button>
            </form>
        </div>
        <div class="submissions-list" id="permitProcessingSubmissions"></div>
    `,
    incidentManagement: `
        <div class="mobile-header">
            <button class="menu-toggle" onclick="toggleSidebar()">
                <i class="fas fa-bars"></i>
            </button>
            <div class="mobile-logo">
                <i class="fas fa-clipboard-list"></i>
                <span>ComplaintHub</span>
            </div>
            <button class="menu-toggle" onclick="logout()">
                <i class="fas fa-sign-out-alt"></i>
            </button>
        </div>
        <h1><i class="fas fa-exclamation-triangle"></i> Incident Management</h1>
        <div class="card">
            <h2><i class="fas fa-flag"></i> Report Incident</h2>
            <form onsubmit="handleSubmit(event, 'incidentManagement')">
                <div class="form-grid">
                    <div class="form-group">
                        <label class="form-label">Location</label>
                        <input type="text" class="form-control" name="location" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Date & Time</label>
                        <input type="datetime-local" class="form-control" name="datetime" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Incident Type</label>
                        <select class="form-control" name="incidentType" required>
                            <option value="">Select Type</option>
                            <option value="accident">Traffic Accident</option>
                            <option value="hazard">Road Hazard</option>
                            <option value="flood">Flooding</option>
                            <option value="power">Power Outage</option>
                            <option value="other">Other</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Incident Description</label>
                        <textarea class="form-control" name="description" rows="4" required></textarea>
                    </div>
                </div>
                <button type="submit" class="btn">
                    <i class="fas fa-paper-plane"></i> Report Incident
                </button>
            </form>
        </div>
        <div class="submissions-list" id="incidentManagementSubmissions"></div>
    `,
    accessibilityServices: `
        <div class="mobile-header">
            <button class="menu-toggle" onclick="toggleSidebar()">
                <i class="fas fa-bars"></i>
            </button>
            <div class="mobile-logo">
                <i class="fas fa-clipboard-list"></i>
                <span>ComplaintHub</span>
            </div>
            <button class="menu-toggle" onclick="logout()">
                <i class="fas fa-sign-out-alt"></i>
            </button>
        </div>
        <h1><i class="fas fa-universal-access"></i> Accessibility Services</h1>
        <div class="card">
            <h2><i class="fas fa-hands-helping"></i> Request Service</h2>
            <form onsubmit="handleSubmit(event, 'accessibilityServices')">
                <div class="form-grid">
                    <div class="form-group">
                        <label class="form-label">Service Type</label>
                        <select class="form-control" name="serviceType" required>
                            <option value="">Select Service</option>
                            <option value="wheelchair">Wheelchair Access</option>
                            <option value="assistance">Personal Assistance</option>
                            <option value="transport">Special Transport</option>
                            <option value="sign">Sign Language</option>
                            <option value="other">Other</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Requester Name</label>
                        <input type="text" class="form-control" name="requesterName" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Contact Information</label>
                        <input type="text" class="form-control" name="contactInfo" required>
                    </div>
                    <div class="form-group">
                        <label class="form-label">Special Requirements</label>
                        <textarea class="form-control" name="requirements" rows="4" required></textarea>
                    </div>
                </div>
                <button type="submit" class="btn">
                    <i class="fas fa-paper-plane"></i> Submit Request
                </button>
            </form>
        </div>
        <div class="submissions-list" id="accessibilityServicesSubmissions"></div>
    `
};

// Main App Template
const appTemplate = `
    <nav class="sidebar" id="sidebar">
        <div class="logo-container">
            <div class="logo">
                <i class="fas fa-clipboard-list"></i>
                <span>ComplaintHub</span>
            </div>
        </div>
        <div class="nav-links" id="navLinks"></div>
        <div style="margin-top: auto; padding: 1rem 2rem; border-top: 1px solid rgba(255, 255, 255, 0.1);">
            <a href="#" onclick="logout()" style="color: white; text-decoration: none; display: flex; align-items: center; gap: 0.5rem;">
                <i class="fas fa-sign-out-alt"></i> Logout
            </a>
        </div>
    </nav>

    <main class="main-content" id="mainContent"></main>
`;

// Navigation Items
const navItems = [
    { id: 'dashboard', icon: 'fas fa-chart-pie', text: 'Dashboard' },
    { id: 'citizenEngagement', icon: 'fas fa-users', text: 'Citizen Engagement' },
    { id: 'complaintResolution', icon: 'fas fa-exclamation-circle', text: 'Complaint Resolution' },
    { id: 'permitProcessing', icon: 'fas fa-file-alt', text: 'Permit Processing' },
    { id: 'incidentManagement', icon: 'fas fa-exclamation-triangle', text: 'Incident Management' },
    { id: 'accessibilityServices', icon: 'fas fa-universal-access', text: 'Accessibility Services' }
];

// Initialize data storage
let submissionsData = JSON.parse(localStorage.getItem('kitsPortalData')) || {
    citizenEngagement: [],
    complaintResolution: [],
    permitProcessing: [],
    incidentManagement: [],
    accessibilityServices: []
};

// User authentication state
let isAuthenticated = JSON.parse(localStorage.getItem('isAuthenticated')) || false;
let currentUser = JSON.parse(localStorage.getItem('currentUser')) || null;
let users = JSON.parse(localStorage.getItem('users')) || [];

// Generate unique case ID
function generateCaseId() {
    const date = new Date();
    const year = date.getFullYear().toString().slice(-2);
    const month = (date.getMonth() + 1).toString().padStart(2, '0');
    const day = date.getDate().toString().padStart(2, '0');
    const random = Math.floor(Math.random() * 10000).toString().padStart(4, '0');
    return `COMP-${year}${month}${day}-${random}`;
}

// Initialize the application
function initializeApp() {
    const appElement = document.getElementById('app');
    
    if (isAuthenticated) {
        // User is authenticated, show the main app
        appElement.innerHTML = appTemplate;
        initializeNavigation();
        loadPage('dashboard');
    } else {
        // User is not authenticated, show the login page
        loadPage('login');
    }
}

function initializeNavigation() {
    const navLinks = document.getElementById('navLinks');
    if (!navLinks) return;
    
    navLinks.innerHTML = '';
    navItems.forEach(item => {
        const link = document.createElement('a');
        link.href = '#';
        link.className = 'nav-link';
        link.innerHTML = `<i class="${item.icon}"></i>${item.text}`;
        link.onclick = () => loadPage(item.id);
        navLinks.appendChild(link);
    });
}

function toggleSidebar() {
    const sidebar = document.getElementById('sidebar');
    const overlay = document.getElementById('sidebarOverlay');
    
    sidebar.classList.toggle('active');
    overlay.classList.toggle('active');
}

function loadPage(pageId) {
    if (pageId === 'login' || pageId === 'signup') {
        // Auth pages don't use the app template
        document.getElementById('app').innerHTML = pageTemplates[pageId];
        
        // Add event listeners for auth forms
        if (pageId === 'login') {
            document.getElementById('loginForm').addEventListener('submit', handleLogin);
        } else if (pageId === 'signup') {
            document.getElementById('signupForm').addEventListener('submit', handleSignup);
        }
    } else if (isAuthenticated) {
        // Make sure the app template is loaded
        if (!document.querySelector('.sidebar')) {
            document.getElementById('app').innerHTML = appTemplate;
            initializeNavigation();
        }
        
        const mainContent = document.getElementById('mainContent');
        mainContent.innerHTML = pageTemplates[pageId] || '<h1>Page not found</h1>';
        
        document.querySelectorAll('.nav-link').forEach(link => {
            link.classList.remove('active');
            if (link.textContent.includes(navItems.find(item => item.id === pageId)?.text)) {
                link.classList.add('active');
            }
        });

        // Initialize page-specific functionality
        if (pageId === 'dashboard') {
            initializeDashboard();
        } else if (pageId === 'complaintResolution') {
            displayComplaintSubmissions();
        } else {
            displaySubmissions(pageId);
        }
    } else {
        // Not authenticated and not on an auth page, redirect to login
        loadPage('login');
    }
}

// Show toast notification
function showToast(message, type = 'success') {
    // Remove any existing toasts
    const existingToasts = document.querySelectorAll('.toast');
    existingToasts.forEach(toast => {
        document.body.removeChild(toast);
    });
    
    // Create new toast
    const toast = document.createElement('div');
    toast.className = `toast toast-${type}`;
    
    let icon = 'check-circle';
    if (type === 'error') icon = 'exclamation-circle';
    if (type === 'info') icon = 'info-circle';
    
    toast.innerHTML = `
        <i class="fas fa-${icon}"></i>
        <div>${message}</div>
    `;
    
    document.body.appendChild(toast);
    
    // Remove toast after 3 seconds
    setTimeout(() => {
        toast.style.animation = 'slideOut 0.3s ease-in forwards';
        setTimeout(() => {
            if (document.body.contains(toast)) {
                document.body.removeChild(toast);
            }
        }, 300);
    }, 3000);
}

// Authentication Functions
function handleLogin(event) {
    event.preventDefault();
    
    // Reset error messages
    document.getElementById('loginEmailError').textContent = '';
    document.getElementById('loginPasswordError').textContent = '';
    
    const email = document.getElementById('loginEmail').value;
    const password = document.getElementById('loginPassword').value;
    
    // Simple validation
    let isValid = true;
    
    if (!email) {
        document.getElementById('loginEmailError').textContent = 'Email is required';
        isValid = false;
    }
    
    if (!password) {
        document.getElementById('loginPasswordError').textContent = 'Password is required';
        isValid = false;
    }
    
    if (!isValid) return;
    
    // Check if user exists
    const user = users.find(u => u.email === email && u.password === password);
    
    if (user) {
        // Login successful
        isAuthenticated = true;
        currentUser = {
            name: user.name,
            email: user.email
        };
        
        localStorage.setItem('isAuthenticated', JSON.stringify(isAuthenticated));
        localStorage.setItem('currentUser', JSON.stringify(currentUser));
        
        // Redirect to dashboard
        initializeApp();
        showToast(`Welcome back, ${user.name}!`);
    } else {
        // Login failed
        document.getElementById('loginPasswordError').textContent = 'Invalid email or password';
    }
}

function handleSignup(event) {
    event.preventDefault();
    
    // Reset error messages
    document.getElementById('signupNameError').textContent = '';
    document.getElementById('signupEmailError').textContent = '';
    document.getElementById('signupPasswordError').textContent = '';
    document.getElementById('signupConfirmPasswordError').textContent = '';
    
    const name = document.getElementById('signupName').value;
    const email = document.getElementById('signupEmail').value;
    const password = document.getElementById('signupPassword').value;
    const confirmPassword = document.getElementById('signupConfirmPassword').value;
    
    // Simple validation
    let isValid = true;
    
    if (!name) {
        document.getElementById('signupNameError').textContent = 'Name is required';
        isValid = false;
    }
    
    if (!email) {
        document.getElementById('signupEmailError').textContent = 'Email is required';
        isValid = false;
    } else if (!email.includes('@')) {
        document.getElementById('signupEmailError').textContent = 'Please enter a valid email';
        isValid = false;
    }
    
    if (!password) {
        document.getElementById('signupPasswordError').textContent = 'Password is required';
        isValid = false;
    } else if (password.length < 6) {
        document.getElementById('signupPasswordError').textContent = 'Password must be at least 6 characters';
        isValid = false;
    }
    
    if (password !== confirmPassword) {
        document.getElementById('signupConfirmPasswordError').textContent = 'Passwords do not match';
        isValid = false;
    }
    
    if (!isValid) return;
    
    // Check if email already exists
    if (users.some(u => u.email === email)) {
        document.getElementById('signupEmailError').textContent = 'Email already in use';
        return;
    }
    
    // Create new user
    const newUser = {
        name,
        email,
        password
    };
    
    users.push(newUser);
    localStorage.setItem('users', JSON.stringify(users));
    
    // Auto login
    isAuthenticated = true;
    currentUser = {
        name: newUser.name,
        email: newUser.email
    };
    
    localStorage.setItem('isAuthenticated', JSON.stringify(isAuthenticated));
    localStorage.setItem('currentUser', JSON.stringify(currentUser));
    
    // Redirect to dashboard
    initializeApp();
    showToast('Account created successfully!');
}

function skipAuth() {
    // Skip authentication and proceed as guest
    isAuthenticated = true;
    currentUser = {
        name: 'Guest User',
        email: 'guest@example.com'
    };
    
    localStorage.setItem('isAuthenticated', JSON.stringify(isAuthenticated));
    localStorage.setItem('currentUser', JSON.stringify(currentUser));
    
    // Redirect to dashboard
    initializeApp();
    showToast('Welcome, Guest User!', 'info');
}

function logout() {
    isAuthenticated = false;
    currentUser = null;
    
    localStorage.setItem('isAuthenticated', JSON.stringify(isAuthenticated));
    localStorage.setItem('currentUser', JSON.stringify(currentUser));
    
    // Redirect to login
    loadPage('login');
    showToast('You have been logged out', 'info');
}

// Dashboard Functions
function initializeDashboard() {
    updateDashboardStats();
    renderStatusChart();
    renderServiceTypeChart();
    displayRecentComplaints();
}

function updateDashboardStats() {
    const complaints = submissionsData.complaintResolution;
    
    const totalComplaints = complaints.length;
    const resolvedComplaints = complaints.filter(c => c.status === 'resolved').length;
    const processingComplaints = complaints.filter(c => c.status === 'processing').length;
    const pendingComplaints = complaints.filter(c => c.status === 'pending').length;
    
    document.getElementById('totalComplaints').textContent = totalComplaints;
    document.getElementById('resolvedComplaints').textContent = resolvedComplaints;
    document.getElementById('processingComplaints').textContent = processingComplaints;
    document.getElementById('pendingComplaints').textContent = pendingComplaints;
}

function renderStatusChart() {
    const complaints = submissionsData.complaintResolution;
    
    const pendingCount = complaints.filter(c => c.status === 'pending').length;
    const processingCount = complaints.filter(c => c.status === 'processing').length;
    const resolvedCount = complaints.filter(c => c.status === 'resolved').length;
    
    const ctx = document.getElementById('statusChart').getContext('2d');
    new Chart(ctx, {
        type: 'pie',
        data: {
            labels: ['Pending', 'Processing', 'Resolved'],
            datasets: [{
                data: [pendingCount, processingCount, resolvedCount],
                backgroundColor: ['#fef3c7', '#dbeafe', '#dcfce7'],
                borderColor: ['#92400e', '#1e40af', '#166534'],
                borderWidth: 1
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
                legend: {
                    position: 'right'
                }
            }
        }
    });
}

function renderServiceTypeChart() {
    const complaints = submissionsData.complaintResolution;
    
    // Count complaints by service type
    const serviceTypes = {};
    complaints.forEach(complaint => {
        const type = complaint.serviceType || 'Other';
        serviceTypes[type] = (serviceTypes[type] || 0) + 1;
    });
    
    const labels = Object.keys(serviceTypes);
    const data = Object.values(serviceTypes);
    
    // Generate colors
    const backgroundColors = [
        '#bfdbfe', '#ddd6fe', '#fecaca', '#bbf7d0', '#fed7aa', '#fef3c7'
    ];
    
    const ctx = document.getElementById('serviceChart').getContext('2d');
    new Chart(ctx, {
        type: 'doughnut',
        data: {
            labels: labels,
            datasets: [{
                data: data,
                backgroundColor: backgroundColors.slice(0, labels.length),
                borderWidth: 1
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
                legend: {
                    position: 'right'
                }
            }
        }
    });
}

function displayRecentComplaints() {
    const recentComplaints = document.getElementById('recentComplaints');
    const complaints = submissionsData.complaintResolution;
    
    // Sort by timestamp (newest first) and take the first 5
    const sortedComplaints = [...complaints]
        .sort((a, b) => new Date(b.timestamp) - new Date(a.timestamp))
        .slice(0, 5);
    
    if (sortedComplaints.length === 0) {
        recentComplaints.innerHTML = '<p class="text-center">No complaints submitted yet</p>';
        return;
    }
    
    recentComplaints.innerHTML = sortedComplaints.map(complaint => `
        <div class="submission-item">
            <div class="case-id">
                ${complaint.caseId}
                <span class="complaint-status status-${complaint.status}">
                    <i class="fas fa-${complaint.status === 'pending' ? 'clock' : complaint.status === 'processing' ? 'spinner' : 'check-circle'}"></i>
                    ${complaint.status.charAt(0).toUpperCase() + complaint.status.slice(1)}
                </span>
            </div>
            <p><strong>Submitted:</strong> ${complaint.timestamp}</p>
            <p><strong>Name:</strong> ${complaint.name}</p>
            <p><strong>Service Type:</strong> ${complaint.serviceType}</p>
            <p><strong>Details:</strong> ${complaint.details.substring(0, 100)}${complaint.details.length > 100 ? '...' : ''}</p>
        </div>
    `).join('');
}

function handleSubmit(event, formType) {
    event.preventDefault();
    const formData = new FormData(event.target);
    const submission = {
        id: Date.now(),
        timestamp: new Date().toLocaleString(),
        ...Object.fromEntries(formData.entries())
    };

    // Add submission to storage
    submissionsData[formType].push(submission);
    localStorage.setItem('kitsPortalData', JSON.stringify(submissionsData));

    // Update display
    displaySubmissions(formType);
    
    event.target.reset();
    
    // Show success message
    showToast(`${formType.replace(/([A-Z])/g, ' $1').trim()} submitted successfully!`);
}

function handleComplaintSubmit(event) {
    event.preventDefault();
    const formData = new FormData(event.target);
    const submission = {
        id: Date.now(),
        caseId: generateCaseId(),
        timestamp: new Date().toLocaleString(),
        status: 'pending',
        ...Object.fromEntries(formData.entries())
    };

    // Add submission to storage
    submissionsData.complaintResolution.push(submission);
    localStorage.setItem('kitsPortalData', JSON.stringify(submissionsData));

    // Update display
    displayComplaintSubmissions();
    
    event.target.reset();
    
    // Show success message
    showToast(`Complaint submitted successfully! Case ID: ${submission.caseId}`);
}

function updateComplaintStatus(caseId, newStatus) {
    const complaintIndex = submissionsData.complaintResolution.findIndex(
        complaint => complaint.caseId === caseId
    );

    if (complaintIndex !== -1) {
        submissionsData.complaintResolution[complaintIndex].status = newStatus;
        localStorage.setItem('kitsPortalData', JSON.stringify(submissionsData));
        displayComplaintSubmissions();
        
        // Update dashboard if we're on that page
        if (document.getElementById('statusChart')) {
            initializeDashboard();
        }
        
        // Show success message
        showToast(`Status updated to ${newStatus}`);
    }
}

function displayComplaintSubmissions() {
    const submissionsList = document.getElementById('complaintResolutionSubmissions');
    if (!submissionsList) return;

    const submissions = submissionsData.complaintResolution;
    
    if (submissions.length === 0) {
        submissionsList.innerHTML = `
            <div class="card">
                <h2><i class="fas fa-history"></i> Previous Complaints</h2>
                <p class="text-center">No complaints submitted yet</p>
            </div>
        `;
        return;
    }
    
    // Sort by timestamp (newest first)
    const sortedSubmissions = [...submissions].sort((a, b) => new Date(b.timestamp) - new Date(a.timestamp));
    
    submissionsList.innerHTML = `
        <div class="card">
            <h2><i class="fas fa-history"></i> Previous Complaints</h2>
            ${sortedSubmissions.map(submission => `
                <div class="submission-item">
                    <div class="case-id">
                        ${submission.caseId}
                        <span class="complaint-status status-${submission.status}">
                            <i class="fas fa-${submission.status === 'pending' ? 'clock' : submission.status === 'processing' ? 'spinner' : 'check-circle'}"></i>
                            ${submission.status.charAt(0).toUpperCase() + submission.status.slice(1)}
                        </span>
                    </div>
                    <p><strong>Submitted:</strong> ${submission.timestamp}</p>
                    <p><strong>Name:</strong> ${submission.name}</p>
                    <p><strong>Contact:</strong> ${submission.contactNumber}</p>
                    <p><strong>Email:</strong> ${submission.email}</p>
                    <p><strong>Service Type:</strong> ${submission.serviceType}</p>
                    <p><strong>Details:</strong> ${submission.details}</p>
                    
                    <div class="complaint-actions">
                        ${submission.status === 'pending' ? `
                            <button class="status-btn status-btn-processing" 
                                onclick="updateComplaintStatus('${submission.caseId}', 'processing')">
                                <i class="fas fa-spinner"></i> Mark as Processing
                            </button>
                        ` : ''}
                        ${submission.status === 'processing' ? `
                            <button class="status-btn status-btn-resolved" 
                                onclick="updateComplaintStatus('${submission.caseId}', 'resolved')">
                                <i class="fas fa-check-circle"></i> Mark as Resolved
                            </button>
                        ` : ''}
                        <button class="delete-btn" onclick="deleteSubmission('complaintResolution', ${submission.id})">
                            <i class="fas fa-trash-alt"></i> Delete
                        </button>
                    </div>
                </div>
            `).join('')}
        </div>
    `;
}

function displaySubmissions(formType) {
    if (formType === 'complaintResolution') {
        displayComplaintSubmissions();
        return;
    }

    const submissionsList = document.getElementById(`${formType}Submissions`);
    if (!submissionsList) return;

    const submissions = submissionsData[formType];
    
    if (submissions.length === 0) {
        submissionsList.innerHTML = `
            <div class="card">
                <h2><i class="fas fa-history"></i> Previous Submissions</h2>
                <p class="text-center">No submissions yet</p>
            </div>
        `;
        return;
    }
    
    // Sort by timestamp (newest first)
    const sortedSubmissions = [...submissions].sort((a, b) => new Date(b.timestamp) - new Date(a.timestamp));
    
    submissionsList.innerHTML = `
        <div class="card">
            <h2><i class="fas fa-history"></i> Previous Submissions</h2>
            ${sortedSubmissions.map(submission => `
                <div class="submission-item">
                    <p><strong>Submitted:</strong> ${submission.timestamp}</p>
                    ${Object.entries(submission)
                        .filter(([key]) => !['id', 'timestamp'].includes(key))
                        .map(([key, value]) => `
                            <p><strong>${key.split(/(?=[A-Z])/).join(' ').replace(/^\w/, c => c.toUpperCase())}:</strong> ${value}</p>
                        `).join('')}
                    <button class="delete-btn" onclick="deleteSubmission('${formType}', ${submission.id})">
                        <i class="fas fa-trash-alt"></i> Delete
                    </button>
                </div>
            `).join('')}
        </div>
    `;
}

function deleteSubmission(formType, submissionId) {
    // Remove submission from storage
    submissionsData[formType] = submissionsData[formType].filter(
        submission => submission.id !== submissionId
    );
    
    // Update localStorage
    localStorage.setItem('kitsPortalData', JSON.stringify(submissionsData));
    
    // Refresh display
    if (formType === 'complaintResolution') {
        displayComplaintSubmissions();
        // Update dashboard if we're on that page
        if (document.getElementById('statusChart')) {
            initializeDashboard();
        }
    } else {
        displaySubmissions(formType);
    }
    
    // Show success message
    showToast('Submission deleted successfully', 'error');
}

// Add event listener for clearing storage (optional, for testing)
function clearAllData() {
    localStorage.removeItem('kitsPortalData');
    submissionsData = {
        citizenEngagement: [],
        complaintResolution: [],
        permitProcessing: [],
        incidentManagement: [],
        accessibilityServices: []
    };
    
    if (document.getElementById('statusChart')) {
        initializeDashboard();
    } else {
        const currentPage = document.querySelector('.nav-link.active')?.textContent.trim();
        const pageId = navItems.find(item => item.text === currentPage)?.id;
        if (pageId) loadPage(pageId);
    }
    
    // Show success message
    showToast('All data has been cleared', 'error');
}

// Initialize the application
document.addEventListener('DOMContentLoaded', () => {
    initializeApp();
});
</script>
</body>
</html>
