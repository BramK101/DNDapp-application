<script lang="ts">
  import "../app.css";

  let email = "";
  let password = "";
  let userName = "";
  let errorMsg = "";

  async function login(event: Event) {
    event.preventDefault();
    errorMsg = "";
    userName = "";

    try {
      const response = await fetch("http://localhost:8080/login", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ email, password }),
      });

      if (!response.ok) {
        throw new Error("Login failed");
      }

      const jsonData = await response.json();

      // Use the correct property from the response
      userName = jsonData.user?.username || "Not found";
    } catch (err) {
      errorMsg = err.message;
    }
  }
</script>

<div class="min-h-screen flex items-center justify-center">
  <fieldset class="fieldset bg-base-200 border-base-300 rounded-box w-xs border p-4">
    <legend class="fieldset-legend text-2xl">Login</legend>
  
    <form on:submit|preventDefault={login}>
      <label class="input validator">
        <svg class="h-[1em] opacity-50" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
          <g
            stroke-linejoin="round"
            stroke-linecap="round"
            stroke-width="2.5"
            fill="none"
            stroke="currentColor"
          >
            <rect width="20" height="16" x="2" y="4" rx="2"></rect>
            <path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7"></path>
          </g>
        </svg>
        <input type="email" bind:value={email} required placeholder="Email" />
      </label>
      <div class="validator-hint hidden">Enter valid email address</div>
    
      <label class="input validator">
        <svg class="h-[1em] opacity-50" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
          <g
            stroke-linejoin="round"
            stroke-linecap="round"
            stroke-width="2.5"
            fill="none"
            stroke="currentColor"
          >
            <path
              d="M2.586 17.414A2 2 0 0 0 2 18.828V21a1 1 0 0 0 1 1h3a1 1 0 0 0 1-1v-1a1 1 0 0 1 1-1h1a1 1 0 0 0 1-1v-1a1 1 0 0 1 1-1h.172a2 2 0 0 0 1.414-.586l.814-.814a6.5 6.5 0 1 0-4-4z"
            ></path>
            <circle cx="16.5" cy="7.5" r=".5" fill="currentColor"></circle>
          </g>
        </svg>
        <input
          type="password"
          bind:value={password}
          required
          placeholder="Password"
          minlength="0"
          title="Must be at least 8 characters"
        />
      </label>
      <p class="validator-hint hidden">
        Must be at least 8 characters, including
        <br />At least one number <br />At least one lowercase letter <br />At least one uppercase letter
      </p>
    
      <button class="btn btn-neutral mt-4">Login</button>
    </form>
    {#if userName}
      <p>User ID: {userName}</p>
    {/if}
    {#if errorMsg}
      <p class="text-red-500">{errorMsg}</p>
    {/if}
  </fieldset>
</div>