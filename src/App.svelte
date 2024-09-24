<head>
  <!-- Import the Roboto font from Google Fonts -->
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
  <!-- Import FontAwesome for icons -->
  <script src="https://kit.fontawesome.com/a076d05399.js" crossorigin="anonymous"></script>
</head>

<script>
  let userName = "Varad Parte";
  let currentPage = 'dashboard';
  let activePage = currentPage;

  // Declare selectedActivityType and amount variables
  let selectedActivityType = "Expense";  // Default activity type
  let amount = 0; // Variable to store the amount entered by the user

  // Navigation function
  function navigate(page) {
    currentPage = page;
  }

  function setActivePage(page){
    activePage = page;
  }

  // Get current date and time
  let currentDate = new Date();
  let formattedDate = currentDate.toLocaleDateString(); 
  let formattedTime = currentDate.toLocaleTimeString();

  // Function to update date and time every second
  setInterval(() => {
    currentDate = new Date();
    formattedDate = currentDate.toLocaleDateString();
    formattedTime = currentDate.toLocaleTimeString();
  }, 1000);

  // How long user has been using the interface
  const startDate = new Date("2024-01-01"); 
  let daysSinceStart = Math.floor((currentDate - startDate) / (1000 * 60 * 60 * 24)); 

  // How many days user has been active (hardcoded for now)
  let daysActive = 30; 

  // Mock activities data for today (can be replaced with real data)
  let todayActivities = [
    { type: 'Expense', amount: 100, description: 'Groceries' },
    { type: 'Investment', amount: 200, description: 'Stock Purchase' }
  ];

  // Determine if user has logged activities today
  let hasLoggedToday = todayActivities.length > 0;

  // Financial goals
  let budgetGoal = 2000;
  let spendingGoal = 1500;
  let investmentGoal = 500;
  let userGoals = {
    budget: budgetGoal,
    spending: spendingGoal,
    investment: investmentGoal
  };

  // Activity and log data
  let activities = [
    { type: 'Expense', amount: 0, description: '' },
    { type: 'Investment', amount: 0, description: '' }
  ];
  
  let logs = [];

  // Customization: available and selected activities
  let availableActivities = ["Expense", "Investment", "Savings"];
  let selectedActivities = ["Expense", "Investment"];

  // Function to save new financial goals
  function setGoals() {
    userGoals = {
      budget: budgetGoal,
      spending: spendingGoal,
      investment: investmentGoal
    };
    alert('Goals have been set!');
  }

  // Add activity to the logs
  function addActivity(type, amount, description) {
    activities.push({ type, amount, description });
    logs.push({ type, amount, description });
  }

  // Toggle activity customization
  function toggleActivity(activity) {
    if (selectedActivities.includes(activity)) {
      selectedActivities = selectedActivities.filter(a => a !== activity);
    } else {
      selectedActivities.push(activity);
    }
  }

  // Log activity
  function logActivity(activity) {
    if (activity === "Savings") {
      let amount = prompt("Enter amount saved:");
      addActivity("Savings", Number(amount), "Savings entry");
    }
  }

  // Summary of performance
  let totalSpent = logs.reduce((acc, log) => log.type === 'Expense' ? acc + log.amount : acc, 0);
  let totalInvested = logs.reduce((acc, log) => log.type === 'Investment' ? acc + log.amount : acc, 0);

  function updateSummary() {
    totalSpent = logs.reduce((acc, log) => log.type === 'Expense' ? acc + log.amount : acc, 0);
    totalInvested = logs.reduce((acc, log) => log.type === 'Investment' ? acc + log.amount : acc, 0);
  }
</script>

<!-- Header -->
<header class="page-header">
  <h1>{currentPage.charAt(0).toUpperCase() + currentPage.slice(1)}</h1> <!-- Capitalize the first letter of the page -->
</header>

<!-- Sidebar -->
<aside class="sidebar">
  <!-- Web app logo and name -->
  <div class="sidebar-logo">
    <span>Fin</span>Trac
  </div>

  <!-- Sidebar options -->
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
    
      <!-- Days Active Tile -->
      <div class="tile">
        <i class="fas fa-calendar-check"></i>
        <h3>Activity Stats</h3>
        <p>Days Since Start: {daysSinceStart}</p>
        <p>Days Active: {daysActive}</p>
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
    </div>  
  {/if}

  <!-- Other pages go here based on navigation -->
  {#if currentPage === 'log'}
    <!-- Log Activity Content -->
    <section>
      <h2>Log Your Financial Activities</h2>
      <div>
        <label for="activityType">Activity Type:</label>
        <select id="activityType" bind:value="{selectedActivityType}">
          <option value="Expense">Expense</option>
          <option value="Investment">Investment</option>
          <option value="Savings">Savings</option>
        </select>
        <input type="number" placeholder="Amount" bind:value="{amount}" />
        <button on:click={() => logActivity(selectedActivityType)}>Log Activity</button>
      </div>
    </section>
  {/if}
  
  {#if currentPage === 'entries'}
    <!-- View Previous Entries Content -->
    <section>
      <h2>View Previous Entries</h2>
      <!-- Insert previous entries code here -->
    </section>
  {/if}

  {#if currentPage === 'summary'}
    <!-- Goals & Summary Content -->
    <section>
      <h2>Your Financial Goals & Summary</h2>
      <ul>
        <li>Total Spent: ${totalSpent}</li>
        <li>Total Invested: ${totalInvested}</li>
      </ul>
      <h4>Goal Achievement:</h4>
      <ul>
        <li>{totalSpent <= budgetGoal ? 'Budget goal achieved!' : 'Over budget!'}</li>
        <li>{totalInvested >= investmentGoal ? 'Investment goal achieved!' : 'Investment goal not reached!'}</li>
      </ul>
      <button on:click={updateSummary}>Update Overview</button>
    </section>
  {/if}

  {#if currentPage === 'settings'}
    <!-- Settings Content -->
    <section>
      <h2>Settings</h2>
      <!-- Insert settings code here -->
    </section>
  {/if}

</main>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body {
    font-family: 'Roboto', sans-serif;
    background-color: #f4f4f4;
  }

  /* Sidebar with full height and logo */
  .sidebar {
    background-color: #191414;
    color: white;
    width: 250px;
    height: 100vh; /* Full height */
    position: fixed;
    top: 0;
    left: 0;
    padding: 20px;
    font-family: 'Roboto', sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
    box-sizing: border-box;
    border-radius: 0 20px 20px 0;
  }

  /* Logo and app name styling */
  .sidebar-logo {
    font-size: 1.8rem;
    font-weight: bold;
    margin-bottom: 50px; /* Space between logo and menu options */
  }

  .sidebar-logo span {
    color: #1db954; /* Logo color for 'Fin' */
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
    transition: color 0.3s ease, text-shadow 0.3s ease; /* Smooth transition for hover */
  }

  .sidebar li:hover {
    color: #1db954; /* Change font color to green on hover */
    text-shadow: 0 0 6px rgba(29, 185, 84, 0.8); /* Subtle green text glow */
  }

  .sidebar li.active { 
    color: #1db954;
    font-weight: bold;
  }

  /* Main Content Area */
  .main-content {
    margin-left: 100px; /* Adjust for wider sidebar */
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
    word-wrap: break-word; /* Ensures long text breaks and wraps inside the tile */
  }

  .dashboard-grid {
    display: flex;
    justify-content: space-around;
    align-items: center;
    flex-wrap: wrap;
    gap: 10px;
    width: 100%;  /* Ensures the grid takes the available width */
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


</style>
