# Script Redirection

```html
<script>
  const domains = ["example1.com", "example2.com", "example3.com"]; // Replace domains with your own

  function getCurrentDomain() {
    return window.location.hostname;
  }

  function redirectOnButtonClick(buttonId) {
    const button = document.getElementById(buttonId);
    if (!button) {
      console.error(`Button with ID "${buttonId}" not found.`);
      return;
    }

    let currentIndex = domains.findIndex(
      (domain) => getCurrentDomain() === domain
    );
    if (currentIndex === -1) {
      console.warn(
        `Current domain "${getCurrentDomain()}" not found in the list. Defaulting to first domain.`
      );
      currentIndex = 0; // Default to first domain if not found
    }

    const alias = localStorage.getItem("alias");
    console.log(
      alias ? "Alias found: " + alias : "No alias found in localStorage."
    );

    button.addEventListener("click", (event) => {
      event.preventDefault();

      if (currentIndex < domains.length - 1) {
        currentIndex++;
        const domain = domains[currentIndex];
        const redirectUrl = alias
          ? `https://${domain}/${alias}`
          : `https://${domain}`;

        console.log(`Redirecting to: ${redirectUrl}`);
        window.location.href = redirectUrl;
      } else {
        console.log("No more redirects available.");
        button.disabled = true;
        document.getElementById("message").textContent =
          "No more domains to redirect to.";
      }
    });
  }

  redirectOnButtonClick("next"); // Replace your button id
</script>
```

**Example HTML Button**

Here's an example of how you might structure your HTML button:

```html
<button type="button" id="next">Click to Redirect</button>
```

**Considerations**

- Button ID: Ensure the button ID in your HTML (next) matches what you use in the JavaScript.
- Local Storage: The script checks for an alias in local storage, so you may want to set this up if needed:

  ```javascript
  localStorage.setItem("alias", "yourAliasHere");
  ```

- Console Logging: The script includes console logging for debugging; this can be helpful for tracking which domain you’re on and the actions taken.

**Use Case**

This script could be useful for applications where you want to manage user flow through a set of domains, perhaps for promotional landing pages, multiple regional sites, or other similar scenarios.

Feel free to modify the domains or enhance the functionality as per your needs!
