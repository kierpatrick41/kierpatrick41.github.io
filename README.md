<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Personal Expense Tracker</title>
    <!-- Load Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f3f4f6; /* Light gray background */
        }
        /* Custom scrollbar style for better aesthetics (optional, but nice) */
        .custom-scroll::-webkit-scrollbar {
            width: 6px;
        }
        .custom-scroll::-webkit-scrollbar-thumb {
            background-color: #9ca3af;
            border-radius: 3px;
        }
        .custom-scroll::-webkit-scrollbar-track {
            background-color: #e5e7eb;
        }
    </style>
</head>
<body class="min-h-screen antialiased text-gray-800">

    <!-- Header & Navigation -->
    <header class="shadow-lg bg-white sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4 flex justify-between items-center">
            <h1 class="text-2xl font-extrabold text-indigo-600 tracking-tight">
                Budget Buddy
            </h1>
            <div id="auth-controls" class="flex items-center space-x-4">
                <span id="user-name" class="text-sm font-medium text-gray-600 hidden"></span>
                <button id="sign-out-button" onclick="signOutUser()" class="hidden bg-gray-200 text-gray-700 text-sm font-medium py-2 px-4 rounded-lg hover:bg-gray-300 transition duration-150">
                    Sign Out
                </button>
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto p-4 sm:p-6 lg:p-8">
        <!-- Message Box for Alerts and Feedback -->
        <div id="message-box" class="mb-4"></div>

        <!-- Login Screen -->
        <section id="login-screen" class="flex flex-col items-center justify-center min-h-[70vh] text-center bg-white rounded-xl shadow-2xl p-8 transition-all">
            <div class="max-w-md">
                <h2 class="text-4xl font-bold mb-4 text-indigo-700">Welcome to Budget Buddy</h2>
                <p class="text-lg text-gray-600 mb-8">
                    Log in with Google to securely track and summarize your expenses in real-time.
                </p>
                <button onclick="signInWithGoogle()" class="bg-indigo-600 text-white font-bold py-3 px-8 rounded-lg shadow-xl hover:bg-indigo-700 transition duration-300 flex items-center justify-center space-x-2 mx-auto">
                    <!-- Google Icon SVG -->
                    <svg class="w-5 h-5" viewBox="0 0 24 24" fill="currentColor" xmlns="http://www.w3.org/2000/svg"><path d="M12 0C5.373 0 0 5.373 0 12c0 5.303 3.438 9.8 8.205 11.385.6.11.82-.26.82-.577 0-.285-.01-1.04-.015-2.04-3.344.725-4.04-1.612-4.04-1.612-.546-1.38-1.332-1.745-1.332-1.745-1.087-.745.084-.73.084-.73 1.205.084 1.838 1.238 1.838 1.238 1.07 1.833 2.809 1.305 3.495.998.108-.775.419-1.305.762-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.467-2.383 1.238-3.224-.125-.3-.538-1.524.118-3.18 0 0 1.008-.323 3.301 1.23.96-.267 1.98-.4 3-.404 1.02.004 2.04.137 3 .404 2.29-1.553 3.298-1.23 3.298-1.23.656 1.656.244 2.88.118 3.18.77.84 1.238 1.914 1.238 3.224 0 4.608-2.805 5.625-5.474 5.922.43.37.817 1.104.817 2.227 0 1.605-.015 2.89-.015 3.284 0 .32.215.69.825.57C20.565 21.802 24 17.303 24 12c0-6.627-5.373-12-12-12z" fill="#FFFFFF"/></svg>
                    <span>Sign In with Google</span>
                </button>
            </div>
        </section>

        <!-- Main Application Screen (Hidden until authenticated) -->
        <section id="main-app" class="hidden grid grid-cols-1 lg:grid-cols-3 gap-8">

            <!-- Column 1: Add New Expense -->
            <div class="lg:col-span-1 bg-white p-6 rounded-xl shadow-xl h-fit">
                <h2 class="text-3xl font-bold mb-6 text-indigo-700">Log New Expense</h2>
                <form id="expense-form" class="space-y-4">
                    <!-- Date Input -->
                    <div>
                        <label for="expense-date" class="block text-sm font-medium text-gray-700 mb-1">Date</label>
                        <input type="date" id="expense-date" required class="w-full p-3 border border-gray-300 rounded-lg focus:ring-indigo-500 focus:border-indigo-500">
                    </div>
                    <!-- Amount Input -->
                    <div>
                        <label for="expense-amount" class="block text-sm font-medium text-gray-700 mb-1">Amount ($)</label>
                        <input type="number" step="0.01" min="0.01" id="expense-amount" placeholder="e.g., 45.99" required class="w-full p-3 border border-gray-300 rounded-lg focus:ring-indigo-500 focus:border-indigo-500">
                    </div>
                    <!-- Description Input -->
                    <div>
                        <label for="expense-description" class="block text-sm font-medium text-gray-700 mb-1">Description</label>
                        <input type="text" id="expense-description" placeholder="e.g., Groceries at Safeway" required class="w-full p-3 border border-gray-300 rounded-lg focus:ring-indigo-500 focus:border-indigo-500">
                    </div>
                    <!-- Category Input -->
                    <div>
                        <label for="expense-category" class="block text-sm font-medium text-gray-700 mb-1">Category</label>
                        <select id="expense-category" required class="w-full p-3 border border-gray-300 rounded-lg focus:ring-indigo-500 focus:border-indigo-500 bg-white">
                            <option value="Food">Food & Dining</option>
                            <option value="Housing">Housing</option>
                            <option value="Transport">Transportation</option>
                            <option value="Bills">Utilities & Bills</option>
                            <option value="Entertainment">Entertainment</option>
                            <option value="Other">Other</option>
                        </select>
                    </div>
                    <!-- Submit Button -->
                    <button type="submit" class="w-full bg-indigo-600 text-white font-bold py-3 rounded-lg hover:bg-indigo-700 transition duration-300 shadow-md transform hover:scale-[1.01]">
                        Add Expense
                    </button>
                </form>
            </div>

            <!-- Column 2 & 3: Monthly Summary and Recent Expenses -->
            <div class="lg:col-span-2 space-y-8">

                <!-- Monthly Summary -->
                <div class="bg-white p-6 rounded-xl shadow-xl">
                    <h2 class="text-3xl font-bold mb-6 text-indigo-700 flex justify-between items-center">
                        Monthly Expense Summary
                        <span class="text-sm font-normal text-gray-500">Total by Month</span>
                    </h2>
                    <div id="monthly-summary-container" class="space-y-3 max-h-80 overflow-y-auto custom-scroll">
                        <!-- Summary items will be injected here by JavaScript -->
                        <p class="text-gray-500 italic text-center py-4">Loading...</p>
                    </div>
                </div>

                <!-- Recent Expenses List -->
                <div class="bg-white p-6 rounded-xl shadow-xl">
                    <h2 class="text-3xl font-bold mb-6 text-indigo-700">Recent Transactions</h2>
                    <div id="expenses-list-container" class="divide-y divide-gray-100 max-h-96 overflow-y-auto custom-scroll">
                        <!-- Expense list items will be injected here by JavaScript -->
                        <p class="text-gray-500 italic text-center py-4">Loading...</p>
                    </div>
                </div>

            </div>
        </section>
    </main>

    <!-- Footer -->
    <footer class="bg-gray-900 text-white py-4 mt-8">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 text-center text-xs">
            &copy; 2025 Budget Buddy. Data stored securely in Firestore.
        </div>
    </footer>


    <!-- JavaScript for Firebase and Application Logic -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, GoogleAuthProvider, signInWithPopup, signOut, onAuthStateChanged, signInAnonymously, signInWithCustomToken } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, onSnapshot, query, doc, deleteDoc, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
        import { setLogLevel } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
        
        // Enable Firebase debug logging
        setLogLevel('debug');

        // --- Firebase Initialization ---
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');

        // Initialize Firebase App and Services
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        const auth = getAuth(app);
        
        let currentUserId = null;

        // --- Authentication Logic ---

        const signInWithGoogle = async () => {
            const provider = new GoogleAuthProvider();
            try {
                // Optional: Force account selection every time
                // provider.setCustomParameters({ prompt: 'select_account' });
                await signInWithPopup(auth, provider);
            } catch (error) {
                console.error("Google Sign-In Failed:", error);
                document.getElementById('message-box').innerHTML = '<p class="p-3 bg-red-100 text-red-700 rounded-lg">Google Sign-In failed: ' + error.message + '</p>';
            }
        };

        const signOutUser = async () => {
            try {
                await signOut(auth);
                document.getElementById('message-box').innerHTML = '<p class="p-3 bg-green-100 text-green-700 rounded-lg">You have been signed out.</p>';
            } catch (error) {
                console.error("Sign Out Failed:", error);
            }
        };

        // Auth State Listener to handle UI changes
        onAuthStateChanged(auth, (user) => {
            const loginScreen = document.getElementById('login-screen');
            const mainApp = document.getElementById('main-app');
            const userNameDisplay = document.getElementById('user-name');
            const signOutButton = document.getElementById('sign-out-button');
            const authControls = document.getElementById('auth-controls');

            if (user) {
                currentUserId = user.uid;
                userNameDisplay.textContent = 'Welcome, ' + (user.displayName || user.email || 'User');
                
                // Show main app
                loginScreen.classList.add('hidden');
                mainApp.classList.remove('hidden');
                userNameDisplay.classList.remove('hidden');
                signOutButton.classList.remove('hidden');

                // Start listening to data once logged in
                setupExpenseListener();
            } else {
                currentUserId = null;

                // Show login screen
                loginScreen.classList.remove('hidden');
                mainApp.classList.add('hidden');
                userNameDisplay.classList.add('hidden');
                signOutButton.classList.add('hidden');
                
                // Attempt silent sign-in with initial token or anonymously for canvas environment setup
                if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                     signInWithCustomToken(auth, __initial_auth_token).catch((e) => {
                         // Fallback to anonymous if token fails
                         console.error("Custom token sign in failed:", e);
                         signInAnonymously(auth).catch(err => console.error("Anonymous sign in failed:", err));
                     });
                } else {
                    signInAnonymously(auth).catch(err => console.error("Anonymous sign in failed:", err));
                }
            }
        });
        
        // Expose functions globally for HTML calls
        window.signInWithGoogle = signInWithGoogle;
        window.signOutUser = signOutUser;


        // --- Firestore Data Functions ---

        // Helper to construct the private collection path for the user
        const getUserExpensesCollection = (userId) => {
            // Path: /artifacts/{appId}/users/{userId}/expenses
            return collection(db, `artifacts/${appId}/users/${userId}/expenses`);
        };

        // Add Expense to Firestore
        const addExpense = async (e) => {
            e.preventDefault();
            if (!currentUserId) {
                document.getElementById('message-box').innerHTML = '<p class="p-3 bg-red-100 text-red-700 rounded-lg">Please sign in to record expenses.</p>';
                return;
            }

            const dateInput = document.getElementById('expense-date');
            const amountInput = document.getElementById('expense-amount');
            const descriptionInput = document.getElementById('expense-description');
            const categoryInput = document.getElementById('expense-category');

            const date = dateInput.value;
            const amount = parseFloat(amountInput.value);
            const description = descriptionInput.value.trim();
            const category = categoryInput.value;

            if (!date || isNaN(amount) || amount <= 0 || description === '' || !category) {
                document.getElementById('message-box').innerHTML = '<p class="p-3 bg-yellow-100 text-yellow-700 rounded-lg">Please fill out all fields correctly.</p>';
                return;
            }

            try {
                await addDoc(getUserExpensesCollection(currentUserId), {
                    userId: currentUserId,
                    date: date, // YYYY-MM-DD
                    amount: amount,
                    description: description,
                    category: category,
                    timestamp: serverTimestamp() // Used for server-side ordering/indexing (optional but good practice)
                });
                document.getElementById('message-box').innerHTML = '<p class="p-3 bg-green-100 text-green-700 rounded-lg">Expense added successfully!</p>';
                
                // Clear form fields
                amountInput.value = '';
                descriptionInput.value = '';
                // dateInput.value is intentionally kept for quick entry
            } catch (error) {
                console.error("Error adding document: ", error);
                document.getElementById('message-box').innerHTML = '<p class="p-3 bg-red-100 text-red-700 rounded-lg">Error adding expense: ' + error.message + '</p>';
            }
        };
        
        // Delete Expense from Firestore
        window.deleteExpense = async (docId) => {
            if (!currentUserId) return;
            
            // NOTE: Using window.confirm() here, but for production, this should be replaced 
            // with a custom modal UI as alerts are disruptive in the sandbox.
            if (window.confirm("Are you sure you want to delete this expense?")) {
                try {
                    const docRef = doc(getUserExpensesCollection(currentUserId), docId);
                    await deleteDoc(docRef);
                    console.log("Document successfully deleted!");
                } catch (error) {
                    console.error("Error removing document: ", error);
                    document.getElementById('message-box').innerHTML = '<p class="p-3 bg-red-100 text-red-700 rounded-lg">Error deleting expense: ' + error.message + '</p>';
                }
            }
        };

        // Real-time Listener for Expenses
        const setupExpenseListener = () => {
            if (!currentUserId) return;

            const expensesRef = getUserExpensesCollection(currentUserId);
            // Query for all documents (we sort/aggregate client-side to avoid Firestore index issues)
            const q = query(expensesRef); 

            onSnapshot(q, (snapshot) => {
                const expensesData = [];
                snapshot.forEach((doc) => {
                    expensesData.push({ id: doc.id, ...doc.data() });
                });

                // 1. Sort by date (newest first)
                expensesData.sort((a, b) => new Date(b.date) - new Date(a.date));
                
                // 2. Calculate Monthly Summaries
                const monthlySummary = {};
                const allMonths = []; 
                
                expensesData.forEach(expense => {
                    // Ensure date is treated as UTC to prevent timezone shifts from changing the day
                    const dateParts = expense.date.split('-'); // expense.date is YYYY-MM-DD
                    const date = new Date(Date.UTC(dateParts[0], dateParts[1] - 1, dateParts[2])); 
                    
                    const yearMonth = date.getUTCFullYear() + '-' + String(date.getUTCMonth() + 1).padStart(2, '0');
                    const monthName = date.toLocaleDateString('en-US', { month: 'long', year: 'numeric', timeZone: 'UTC' });

                    if (!monthlySummary[yearMonth]) {
                        monthlySummary[yearMonth] = { total: 0, monthName: monthName };
                        allMonths.push(yearMonth);
                    }
                    monthlySummary[yearMonth].total += expense.amount;
                });
                
                // Sort allMonths in descending order (most recent first)
                allMonths.sort().reverse();

                // 3. Render
                renderExpensesList(expensesData);
                renderMonthlySummary(monthlySummary, allMonths);
            }, (error) => {
                console.error("Error fetching documents: ", error);
                document.getElementById('message-box').innerHTML = '<p class="p-3 bg-red-100 text-red-700 rounded-lg">Failed to load expenses: ' + error.message + '</p>';
            });
        };

        // --- UI Rendering Functions ---

        const formatCurrency = (amount) => {
            // Check if amount is a number before formatting
            if (typeof amount !== 'number' || isNaN(amount)) {
                return '$0.00';
            }
            return amount.toLocaleString('en-US', { style: 'currency', currency: 'USD' });
        };

        const renderExpensesList = (expenses) => {
            const listContainer = document.getElementById('expenses-list-container');
            if (expenses.length === 0) {
                listContainer.innerHTML = '<p class="text-gray-500 italic text-center py-4">No expenses recorded yet.</p>';
                return;
            }

            let html = '';
            expenses.forEach(expense => {
                const dateDisplay = new Date(expense.date).toLocaleDateString('en-US', {
                    year: 'numeric', month: 'short', day: 'numeric'
                });
                
                html += `
                    <div class="flex justify-between items-center p-3 border-b border-gray-100 last:border-b-0 hover:bg-gray-50 transition duration-100">
                        <div class="flex flex-col flex-1 min-w-0 pr-4">
                            <span class="font-semibold text-gray-800 truncate">${expense.description}</span>
                            <span class="text-xs text-gray-500">${dateDisplay} | ${expense.category}</span>
                        </div>
                        <div class="flex items-center space-x-3">
                            <span class="font-bold text-red-600">${formatCurrency(expense.amount)}</span>
                            <button onclick="deleteExpense('${expense.id}')" aria-label="Delete Expense" class="text-red-400 hover:text-red-600 transition duration-150 p-1 rounded-full hover:bg-red-100">
                                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"></path></svg>
                            </button>
                        </div>
                    </div>
                `;
            });
            listContainer.innerHTML = html;
        };

        const renderMonthlySummary = (summary, orderedMonths) => {
            const summaryContainer = document.getElementById('monthly-summary-container');
            if (orderedMonths.length === 0) {
                summaryContainer.innerHTML = '<p class="text-gray-500 italic text-center py-4">Summary will appear here after you add expenses.</p>';
                return;
            }

            let html = '';
            orderedMonths.forEach(yearMonth => {
                const data = summary[yearMonth];
                html += `
                    <div class="flex justify-between items-center bg-gray-50 p-4 rounded-xl shadow-sm border-l-4 border-indigo-500">
                        <span class="text-lg font-semibold text-gray-700">${data.monthName}</span>
                        <span class="text-xl font-extrabold text-indigo-600">${formatCurrency(data.total)}</span>
                    </div>
                `;
            });
            summaryContainer.innerHTML = html;
        };

        // --- Initial Setup ---

        window.onload = function() {
            // Set today's date as default for the input field
            const dateInput = document.getElementById('expense-date');
            if (dateInput) {
                const today = new Date();
                const yyyy = today.getFullYear();
                const mm = String(today.getMonth() + 1).padStart(2, '0'); // Months are 0-indexed
                const dd = String(today.getDate()).padStart(2, '0');
                dateInput.value = `${yyyy}-${mm}-${dd}`;
            }

            // Attach event listener for form submission
            const form = document.getElementById('expense-form');
            if (form) {
                form.addEventListener('submit', addExpense);
            }
            
            // Clear loading text in summary/list containers
            document.getElementById('monthly-summary-container').innerHTML = '';
            document.getElementById('expenses-list-container').innerHTML = '';
        };

    </script>
</body>
</html>
