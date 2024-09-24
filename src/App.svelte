<head>
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
  <script src="https://kit.fontawesome.com/a076d05399.js" crossorigin="anonymous"></script>
</head>

<script>
  let userName = "Varad Parte";
  let currentPage = 'dashboard';
  let activePage = currentPage;

  let selectedActivityType = "Expense";  // Default activity type
  let amount = 0; // Variable to store the amount entered by the user
  let description = ""; // New description variable for activities
  let image = null; // Variable to hold the uploaded image
  let showMessage = false; // Controls the display of the success message

  function navigate(page) {
    currentPage = page;
  }

  function setActivePage(page){
    activePage = page;
  }

  let currentDate = new Date();
  let formattedDate = currentDate.toLocaleDateString();
  let formattedTime = currentDate.toLocaleTimeString();
  let currentDateString = currentDate.toLocaleDateString('en-CA');

  setInterval(() => {
    currentDate = new Date();
    formattedDate = currentDate.toLocaleDateString();
    formattedTime = currentDate.toLocaleTimeString();
  }, 1000);

  const startDate = new Date("2024-01-01"); 
  let daysSinceStart = Math.floor((currentDate - startDate) / (1000 * 60 * 60 * 24)); 
  let daysActive = 30; 

  // Activity log data stored temporarily
  let todayActivities = [
    { type: 'Expense', amount: 100, description: 'Groceries' },
    { type: 'Investment', amount: 300, description: 'Stock Purchase' }
  ];

  let hasLoggedToday = todayActivities.length > 0;

  function handleImageUpload(event) {
    const file = event.target.files[0];
    if (file) {
      image = file;
    }
  }

  function logActivity() {
    if (amount > 0 && description.trim() !== "") {
      // Log the activity
      todayActivities.push({ 
        type: selectedActivityType, 
        amount: Number(amount), 
        description: description, 
        image: image ? image.name : null // Store the image name if provided
      });

      // Reset input fields
      amount = 0;
      description = "";
      image = null;

      // Show the success message
      showMessage = true;

      // Hide the message after a few seconds
      setTimeout(() => {
        showMessage = false;
      }, 3000);
    } else {
      alert("Please enter both a valid amount and a description.");
    }
  }

  //---------------------------- Previous Entries Page ------------------------------

  let selectedDayEntries = [];  // Entries for the selected day
  let editingEntry = null;  // Tracks which entry is being edited

  // Mock data for previous entries
  let entries = [
    {
      date: "2024-09-20",
      logs: [
        { type: 'Expense', amount: 50, description: 'Lunch' },
        { type: 'Investment', amount: 300, description: 'Stocks' }
      ]
    },
    {
      date: "2024-09-21",
      logs: [
        { type: 'Expense', amount: 30, description: 'Gas' },
        { type: 'Savings', amount: 100, description: 'Savings Deposit' }
      ]
    }
  ];

  // Load entries for the currently selected day
  function loadEntriesForDay() {
    const entryForDay = entries.find(entry => entry.date === currentDateString);
    selectedDayEntries = entryForDay ? entryForDay.logs : [];
  }

  // Edit an entry
  function editEntry(index) {
    editingEntry = index;
  }

  // Save the edited entry
  function saveEntry(index) {
    editingEntry = null;
    alert('Entry updated successfully!');
  }

  // Navigate to the previous day
  function prevDay() {
    currentDate.setDate(currentDate.getDate() - 1);
    currentDateString = currentDate.toISOString().split('T')[0];  // Correct date format
    loadEntriesForDay();
  }

  // Navigate to the next day
  function nextDay() {
    currentDate.setDate(currentDate.getDate() + 1);
    currentDateString = currentDate.toISOString().split('T')[0];  // Correct date format
    loadEntriesForDay();
  }

  // Initialize page with current day entries
  loadEntriesForDay();
  
  
  
  //---------------------------- Summary Page ------------------------------
  // Summary updates (mock for now)
  // Data for user goals
  let userGoals = [
    { activity: 'Expense', target: 1000 },
    { activity: 'Savings', target: 500 },
    { activity: 'Investment', target: 500 }
  ];

  // Mock data for logs
  let logs = [
    { type: 'Expense', amount: 200, date: '2024-09-20' },
    { type: 'Savings', amount: 100, date: '2024-09-21' },
    { type: 'Investment', amount: 50, date: '2024-09-22' }
  ];

  let availableActivities = ['Expense', 'Savings', 'Investment'];
  let selectedActivities = ['Expense', 'Savings', 'Investment']; // Initially selected

  let showSuccessMessage = false; // To show success message upon saving goals/customization

  // Save goals
  function saveGoals() {
    showSuccessMessage = true;
    setTimeout(() => {
      showSuccessMessage = false;
    }, 3000); // Hide success message after 3 seconds
  }

  // Calculate progress toward goals (mock logic for now)
  function calculateGoalProgress(goal) {
    let loggedAmount = logs
      .filter(log => log.type === goal.activity)
      .reduce((acc, log) => acc + log.amount, 0);
    return (loggedAmount / goal.target) * 100; // Return percentage
  }

  // Mock summary data (average expenses, savings, etc.)
  let totalLogs = logs.length;
  let averageExpenses = logs
    .filter(log => log.type === 'Expense')
    .reduce((acc, log) => acc + log.amount, 0) / totalLogs;
  let averageSavings = logs
    .filter(log => log.type === 'Savings')
    .reduce((acc, log) => acc + log.amount, 0) / totalLogs;

  // Toggle activity selection
  function toggleActivity(activity) {
    if (selectedActivities.includes(activity)) {
      selectedActivities = selectedActivities.filter(a => a !== activity);
    } else {
      selectedActivities.push(activity);
    }
  }

  // Save customization
  function saveCustomization() {
    showSuccessMessage = true;
    setTimeout(() => {
      showSuccessMessage = false;
    }, 3000);
  }

  let isDarkTheme = false; // Default is light theme

  function toggleTheme() {
    if (isDarkTheme) {
      document.documentElement.setAttribute('data-theme', 'dark');
    } else {
      document.documentElement.setAttribute('data-theme', 'light');
    }
  }

  // Watch for changes in isDarkTheme and apply the theme
  $: toggleTheme();
</script>

<!-- Header -->
<header class="page-header">
  <h1>{currentPage.charAt(0).toUpperCase() + currentPage.slice(1)}</h1> 
</header>

<!-- Sidebar -->
<aside class="sidebar">
  <div class="sidebar-logo">
    <span>Fin</span>Trac
  </div>

  <ul>
    <li on:click={() => navigate('dashboard')}>Dashboard</li>
    <li on:click={() => navigate('log')}>Log Activity</li>
    <li on:click={() => navigate('entries')}>Previous Entries</li>
    <li on:click={() => navigate('summary')}>Goals & Summary</li>
    <li on:click={() => navigate('settings')}>Settings</li>
  </ul>
</aside>

<!-- Main Content Area -->
<main class="main-content">
  {#if currentPage === 'dashboard'}
  <!-- Dashboard Content -->
  <div class="dashboard-grid">
    <!-- User Overview Tile -->
    <div class="tile">
      <i class="fas fa-user"></i>
      <h3>User Overview</h3>
      <p>Name: {userName}</p>
      <p>Days Since Start: {daysSinceStart}</p>
      <p>Days Active: {daysActive}</p>
    </div>
    
    <!-- Date/Time Tile -->
    <div class="tile">
      <i class="fas fa-clock"></i>
      <h3>Current Date & Time</h3>
      <p>Date: {formattedDate}</p>
      <p>Time: {formattedTime}</p>
    </div>

    <!-- Today's Activities Tile -->
    <div class="tile">
      <i class="fas fa-tasks"></i>
      <h3>Today's Activities</h3>
      {#each todayActivities as activity}
        <p>{activity.type}: ${activity.amount} - {activity.description}</p>
      {/each}
      {#if hasLoggedToday}
        <p class="completed">You've completed your logging for today! <i class="fas fa-check-circle"></i></p>
      {/if}
    </div>

    <!-- Goals & Summary Preview Tile -->
    <div class="tile">
      <i class="fas fa-chart-line"></i>
      <h3>Goals Overview</h3>
      {#each userGoals as goal}
        <p><strong>{goal.activity}</strong>: {calculateGoalProgress(goal)}% complete</p>
      {/each}
      <button class="btn-goals" on:click={() => navigate('summary')}>View Full Summary</button>
    </div>
  </div>
  {/if}

  {#if currentPage === 'log'}
    <!-- Log Activity Page -->
    <section>
      <h2>Log Your Financial Activities</h2>
  
      <!-- Log form tile -->
      <div class="log-tile">
        <div class="form-group">
          <label for="activityType">Activity Type:</label>
          <select id="activityType" bind:value="{selectedActivityType}" class="form-control">
            <option value="Expense">Expense</option>
            <option value="Investment">Investment</option>
            <option value="Savings">Savings</option>
          </select>
        </div>
  
        <div class="form-group">
          <label for="amount">Amount:</label>
          <input type="number" id="amount" placeholder="Enter Amount" bind:value="{amount}" class="form-control" />
        </div>
  
        <div class="form-group">
          <label for="description">Description:</label>
          <input type="text" id="description" placeholder="Enter Description" bind:value="{description}" class="form-control" />
        </div>
  
        <!-- Optional image upload -->
        <div class="form-group">
          <label for="receipt">Attach Bill/Receipt (Optional):</label>
          <input type="file" id="receipt" accept="image/*" class="form-control" on:change="{handleImageUpload}" />
        </div>
  
        <button class="btn-log" on:click={logActivity}>Log Activity</button>
      </div>
  
      <!-- Success message -->
      {#if showMessage}
        <div class="success-message">
          Log has been saved! You can view it on the dashboard.
        </div>
      {/if}
    </section>
  {/if}

  {#if currentPage === 'entries'}
  <!-- Previous Entries Page -->
  <section>
    <h2>Previous Financial Entries</h2>

    <!-- Navigation for Previous/Next days -->
    <div class="nav-buttons">
      <button on:click={prevDay} class="btn-nav">Previous Day</button>
      <span>{currentDateString}</span>
      <button on:click={nextDay} class="btn-nav">Next Day</button>
    </div>

    <!-- Display entries for the selected day -->
    {#if selectedDayEntries.length > 0}
      <div class="entries-grid">
        {#each selectedDayEntries as entry, index}
          <div class="entry-tile">
            {#if editingEntry === index}
              <!-- Edit mode for the entry -->
              <input type="text" bind:value={entry.description} class="form-control" placeholder="Description"/>
              <input type="number" bind:value={entry.amount} class="form-control" placeholder="Amount"/>
              <select bind:value={entry.type} class="form-control">
                <option value="Expense">Expense</option>
                <option value="Investment">Investment</option>
                <option value="Savings">Savings</option>
              </select>
              <button class="btn-save" on:click={() => saveEntry(index)}>Save</button>
            {:else}
              <!-- Display mode for the entry -->
              <p><strong>{entry.type}</strong>: ${entry.amount}</p>
              <p>{entry.description}</p>
              <button class="btn-edit" on:click={() => editEntry(index)}>Edit</button>
            {/if}
          </div>
        {/each}
      </div>
    {:else}
      <p>No entries found for this day.</p>
    {/if}
  </section>
  {/if}
  

  {#if currentPage === 'summary'}
  <section>
    <h2>Goals & Summary</h2>

    <!-- Goal Setting Section -->
    <div class="goal-setting">
      <h3>Set Your Goals</h3>
      <div class="goal-entry">
        {#each userGoals as goal, index}
          <div class="goal-tile">
            <label>{goal.activity} Goal:</label>
            <input type="number" placeholder="Set goal" bind:value="{goal.target}" class="form-control" />
          </div>
        {/each}
      </div>
      <button class="btn-save-goals" on:click={saveGoals}>Save Goals</button>
    </div>

    <!-- Summary Section -->
    <div class="summary-section">
      <h3>Performance Overview</h3>

      {#each userGoals as goal}
        <div class="summary-tile">
          <h4>{goal.activity} Progress</h4>
          <progress max="100" value="{calculateGoalProgress(goal)}"></progress>
          <p>{goal.activity} Target: ${goal.target}</p>
          <p>Progress: ${calculateGoalProgress(goal)}%</p>
        </div>
      {/each}

      <!-- Additional performance overview such as averages and frequency of logging -->
      <h4>Overall Activity Summary</h4>
      <p>Total Activities Logged: {totalLogs}</p>
      <p>Average Expenses: ${averageExpenses}</p>
      <p>Average Savings: ${averageSavings}</p>
    </div>

    <!-- Customization Section -->
    <div class="customization">
      <h3>Customize Activities</h3>

      <h4>Available Activities</h4>
      <ul>
        {#each availableActivities as activity}
          <li>
            <input type="checkbox" checked={selectedActivities.includes(activity)} on:change={() => toggleActivity(activity)} /> 
            {activity}
          </li>
        {/each}
      </ul>

      <button class="btn-save-customization" on:click={saveCustomization}>Save Customization</button>
    </div>

    <!-- Success Message -->
    {#if showSuccessMessage}
      <div class="success-message">
        Your goals and settings have been updated successfully!
      </div>
    {/if}
  </section>
  {/if}

  {#if currentPage === 'settings'}
  <!-- Settings Page -->
  <section>
    <h2>Settings</h2>

    <!-- Theme Toggle -->
    <div class="theme-toggle">
      <label for="themeSwitch">Toggle Theme</label>
      <input type="checkbox" id="themeSwitch" bind:checked={isDarkTheme} on:change={toggleTheme} />
    </div>
  </section>
  {/if}

</main>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  :root {
    --background-color: #ffffff;
    --text-color: #000000;
    --tile-background-color: #f4f4f4;
    --button-background-color: #1db954;
    --button-text-color: #ffffff;
  }


  .sidebar {
    background-color: #191414;
    color: white;
    width: 250px;
    height: 100vh;
    position: fixed;
    top: 0;
    left: 0;
    padding: 20px;
    font-family: 'Roboto', sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
    border-radius: 0 20px 20px 0;
  }

  .sidebar-logo {
    font-size: 1.8rem;
    font-weight: bold;
    margin-bottom: 50px;
  }

  .sidebar-logo span {
    color: #1db954;
  }

  .sidebar ul {
    list-style-type: none;
    width: 100%;
  }

  .sidebar li {
    padding: 15px;
    cursor: pointer;
    color: white;
    border-bottom: 1px solid #333;
    text-align: center;
    transition: color 0.3s ease, text-shadow 0.3s ease;
  }

  .sidebar li:hover {
    color: #1db954;
    text-shadow: 0 0 6px rgba(29, 185, 84, 0.8);
  }

  .main-content {
    margin-left: 100px;
    padding: 2rem;
    min-height: 100vh;
  }

  .tile {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 20px;
    margin: 10px;
    width: 350px;
    min-height: 200px;
    background-color: #f4f4f4;
    border-radius: 10px;
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
    word-wrap: break-word;
  }

  .dashboard-grid {
    display: flex;
    justify-content: space-around;
    align-items: center;
    flex-wrap: wrap;
    gap: 10px;
    width: 100%;
  }

  .tile i {
    font-size: 30px;
    margin-bottom: 10px;
  }

  .completed {
    color: green;
  }

  .page-header {
    text-align: left;
    padding: 10px;
    background-color: white;
    color: black;
    font-size: 10px;
    font-weight: bold;
    border-bottom: 2px solid #ddd;
    width: 100%;
  }
    /* Center and style the log form */
    .log-tile {
    background-color: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    max-width: 400px;
    margin: 20px auto;
    display: flex;
    flex-direction: column;
    gap: 25px;
  }

  /* Form Group for each input field */
  .form-group {
    display: flex;
    flex-direction: column;
  }

  /* General input field styling */


  /* Button styling */
  .btn-log {
    background-color: #1db954;
    color: white;
    font-size: 16px;
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }

  .btn-log:hover {
    background-color: #148f3d;
  }

  

  .nav-buttons {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
    font-size: 18px;
  }

  .btn-nav {
    background-color: #1db954;
    color: white;
    padding: 10px 15px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }

  .btn-nav:hover {
    background-color: #148f3d;
  }

  .entries-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
    justify-content: center;
  }

  .entry-tile {
    background-color: white;
    padding: 15px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    width: 250px;
    text-align: center;
  }

  .form-control {
    width: 100%;
    padding: 10px;
    margin: 10px 0;
    font-size: 16px;
    border: 1px solid #ddd;
    border-radius: 5px;
  }

  .btn-edit, .btn-save {
    background-color: #1db954;
    color: white;
    border: none;
    padding: 10px;
    border-radius: 5px;
    cursor: pointer;
  }

  .btn-edit:hover, .btn-save:hover {
    background-color: #148f3d;
  }

  section h2 {
    text-align: center;
    margin-bottom: 20px;
    font-size: 24px;
  }

  p {
    margin: 10px 0;
  }

  section {
    padding: 20px;
    text-align: center;
  }

  .goal-setting, .summary-section, .customization {
    margin-bottom: 40px;
  }

  .goal-entry {
    display: flex;
    justify-content: center;
    gap: 20px;
    margin-bottom: 20px;
  }

  .goal-tile {
    background-color: white;
    padding: 15px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    width: 200px;
    text-align: center;
  }

  .summary-tile {
    background-color: white;
    padding: 15px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    margin: 10px 0;
  }

  .btn-save-goals, .btn-save-customization {
    background-color: #1db954;
    color: white;
    padding: 10px 15px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }

  .btn-save-goals:hover, .btn-save-customization:hover {
    background-color: #148f3d;
  }

  .success-message {
    background-color: #d4edda;
    color: #155724;
    padding: 10px;
    border: 1px solid #c3e6cb;
    border-radius: 5px;
    text-align: center;
    margin-top: 20px;
    transition: opacity 0.5s ease;
  }

  /* Dark theme overrides */
  [data-theme="dark"] {
    --background-color: #121212;
    --text-color: #f0f0f0;
    --tile-background-color: #333333;
    --button-background-color: #1db954;
    --button-text-color: #ffffff;
  }

  /* Apply theme variables */
  body {
    background-color: var(--background-color);
    color: var(--text-color);
    transition: background-color 0.3s, color 0.3s;
  }

  .theme-toggle {
    display: flex;
    align-items: center;
    gap: 10px;
  }

  input[type="checkbox"] {
    width: 40px;
    height: 20px;
    -webkit-appearance: none;
    appearance: none;
    background-color: #c6c6c6;
    border-radius: 10px;
    position: relative;
    outline: none;
    cursor: pointer;
    transition: background-color 0.3s;
  }

  input[type="checkbox"]:checked {
    background-color: #1db954;
  }

  input[type="checkbox"]:before {
    content: "";
    position: absolute;
    width: 18px;
    height: 18px;
    border-radius: 50%;
    background-color: white;
    top: 1px;
    left: 1px;
    transition: 0.3s;
  }

  input[type="checkbox"]:checked:before {
    transform: translateX(20px);
  }

</style>
