# Postman & API Testing Reference Guide

A practical cheatsheet covering Postman shortcuts, testing scripts, dynamic variables, Newman CLI, and alternative API testing tools.

---

## 1. Quick Online & Desktop Alternatives

| Tool | Description & URL |
| :--- | :--- |
| **ReqBin** | Fast online REST & SOAP API testing tool — [https://reqbin.com/](https://reqbin.com/) |
| **Hoppscotch** | Open-source, lightweight web-based API client — [https://hoppscotch.io/](https://hoppscotch.io/) |
| **Bruno** | Fast, git-friendly, offline-first open-source API client — [https://www.usebruno.com/](https://www.usebruno.com/) |
| **Thunder Client** | Lightweight REST API client extension inside VS Code |
| **HTTPBin** | Mock backend for testing HTTP requests, headers & status codes — [https://httpbin.org/](https://httpbin.org/) |
| **Webhook.site** | Instant URL to test, inspect, and debug incoming webhooks — [https://webhook.site/](https://webhook.site/) |

---

## 2. Essential Postman Keyboard Shortcuts (Windows)

| Action | Shortcut |
| :--- | :--- |
| **Send Request** | `Ctrl + Enter` |
| **New Request Tab** | `Ctrl + T` |
| **Close Current Tab** | `Ctrl + W` |
| **Force Close Tab** | `Ctrl + Alt + W` |
| **Save Request** | `Ctrl + S` |
| **Save As / Duplicate** | `Ctrl + Shift + S` |
| **Open Postman Console** | `Ctrl + Alt + C` |
| **Beautify JSON/Body** | `Ctrl + B` |
| **Find / Quick Search** | `Ctrl + K` or `Ctrl + G` |
| **Toggle Sidebar** | `Ctrl + \` |
| **Select Environment** | `Ctrl + Shift + E` |

---

## 3. Dynamic Variables (Built-in Faker)

Postman provides built-in random generators using `{{$variableName}}` inside URLs, headers, and request bodies:

```json
{
  "id": "{{$guid}}",
  "name": "{{$randomFullName}}",
  "email": "{{$randomEmail}}",
  "phone": "{{$randomPhoneNumber}}",
  "company": "{{$randomCompanyName}}",
  "city": "{{$randomCity}}",
  "country": "{{$randomCountry}}",
  "avatar": "{{$randomAvatarUrl}}",
  "price": {{$randomPrice}},
  "createdAt": "{{$isoTimestamp}}",
  "timestamp": {{$timestamp}}
}
```

---

## 4. Pre-Request & Test Scripts (JavaScript Snippets)

Add these in the **Tests** or **Pre-request Script** tab.

### Auto-Save Auth Token from Login Response
```javascript
// In the "Tests" tab of your Login / Auth endpoint:
const response = pm.response.json();

if (response.token || response.accessToken) {
    const token = response.token || response.accessToken;
    pm.environment.set("authToken", token);
    console.log("Auth token stored successfully:", token);
}
```

> **Use the token in subsequent requests:**
> Under the **Authorization** tab, choose **Bearer Token** and set the value to `{{authToken}}`.

### Status Code & Response Time Assertions
```javascript
// Verify Status 200 OK
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

// Response time under 500ms
pm.test("Response time is less than 500ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(500);
});
```

### JSON Body Validations
```javascript
pm.test("Validate response body schema", function () {
    const jsonData = pm.response.json();
    
    // Check property existence
    pm.expect(jsonData).to.have.property("success");
    pm.expect(jsonData.success).to.eql(true);
    
    // Check array length
    pm.expect(jsonData.data).to.be.an("array").that.is.not.empty;
});
```

### Dynamic Pre-Request Calculations
```javascript
// In Pre-request Script: Generate a future timestamp or custom header
const now = new Date();
pm.environment.set("currentDate", now.toISOString());

const nonce = Math.floor(Math.random() * 1000000);
pm.environment.set("requestNonce", nonce);
```

---

## 5. Newman CLI (Run Collections in Terminal & CI/CD)

Run Postman collections directly from the command line for automated testing.

### Installation
```bash
npm install -g newman
npm install -g newman-reporter-htmlextra
```

### Common Commands
```bash
# Run exported collection
newman run my_collection.json

# Run with environment variables
newman run my_collection.json -e my_environment.json

# Run with multiple iterations using test data file (CSV or JSON)
newman run my_collection.json -d test_data.json

# Generate HTML Extra report
newman run my_collection.json -e my_environment.json -r cli,htmlextra --reporter-htmlextra-export ./reports/report.html
```

---

## 6. Pro Tips & Common Troubleshooting

- **Self-Signed SSL Errors on Localhost:**
  - If calling `https://localhost:5001` fails with SSL issues, go to **Settings (Gear icon) -> General -> Turn OFF "SSL certificate verification"**.
- **Inspect Network Traffic / Headers:**
  - Always press `Ctrl + Alt + C` to open the **Postman Console** to see the actual raw network request, raw headers sent, and redirects.
- **Collection-Level Auth & Scripts:**
  - Instead of setting Bearer Auth on every request, set it on the parent **Collection** root under the `Authorization` tab. All requests in that collection inherit it automatically.
- **Clear Stale Cookies:**
  - Click **Cookies** link beneath the `Send` button to inspect, add, or delete domain cookies maintained by Postman's session jar.