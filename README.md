# Ollama API Test - OneWord Model

This project demonstrates how to run a custom Ollama model locally and test it using a simple JavaScript fetch request. The setup ensures the Ollama server runs on your local machine and accepts requests from web origins such as browser consoles or grading platforms.

🧠 Overview

- Model name: `oneword`  
- Goal: Generate short, meaningful AI responses to prompts such as “opposite of male”.  
- Backend: Ollama running locally on port `11434`  
- Frontend test: JavaScript `fetch()` API call inside the browser console (graded environment)

🧩 Prerequisites

- Ollama installed on your system  
  Download: https://ollama.ai/download  
- PowerShell (for Windows users)  
- VS Code or terminal access to run commands

⚙️ Setup Instructions

1. Pull the Required Model

   Before creating the local model, download the base model referenced in the Modelfile:
   Run ollama pull model_name (e.g., ollama pull llama3) to download the base model referenced in your Modelfile before running ollama create oneword -f Modelfile.
  
   Then register the Modelfile as a local model named `oneword`:

   ```bash
   ollama create oneword -f Modelfile
   ```

   Custom Modelfile and creating the local model

   If you built a custom model using a Modelfile, your project will include a Modelfile like this:

   ```text
   FROM llama3
   PARAMETER temperature 0.7
   SYSTEM "You are a one-word AI. Reply with only one word."
   ```

   To register that Modelfile as a local model named `oneword`, run:

   ```bash
   ollama create oneword -f Modelfile
   ```

   What that command does:
   - Registers a new local model named `oneword` in your Ollama environment.
   - Builds the model based on the instructions in your `Modelfile`.
   - Makes the model available to the API as `model: 'oneword'`.

   Without running `ollama create ...` Ollama will not know what `oneword` refers to and your fetch request will fail with:
   ```
   Model 'oneword' not found
   ```

   If you are pulling an existing model from Ollama’s library:
   - `ollama pull oneword` is sufficient and you do not need `ollama create`.
   - But since `oneword` in this repo is a custom model (not an official Ollama model), you likely needed to run:
   ```bash
   ollama create oneword -f Modelfile
   ```
   ✅ Yes — running `ollama create oneword -f Modelfile` was necessary for the graded assignment that uses the custom `oneword` model.

1. Allow All Origins (for Grader Console Access)

   In PowerShell, run the following command to allow requests from any browser origin for the current session:

   ```powershell
   $env:OLLAMA_ORIGINS="*"
   ```

   Note: This sets the environment variable temporarily for the current PowerShell session. If you restart the terminal you'll need to set it again.

2. Start the Ollama Server

   Once the variable is set, start the Ollama API server:

   ```bash
   ollama serve
   ```

   The server will start on:

   ```
   http://localhost:11434
   ```

3. Verify the Server

   Check if Ollama is responding:

   ```bash
   curl http://localhost:11434/api/tags
   ```

   You should get a JSON list of installed models (including `oneword`).

🧪 Testing in Browser Console

Once the server is running, go to your graded assignment page → open Console (usually via F12 → Console tab) → paste the following code:

```javascript
// Test 1: Basic fetch
fetch('http://localhost:11434/api/generate', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    model: 'oneword',
    prompt: 'opposite of male',
    stream: false
  })
})
.then(response => response.json())
.then(data => {
  console.log('Response:', data.response);
  console.log('Full data:', data);
})
.catch(error => console.error('Error:', error));
```

Then press Enter. If the setup is correct, you’ll see an AI-generated response in your console.

5. Submit the Assignment

After successful test output:

- Click “Ready” in the assignment interface.  
- Then click “Check” to grade your submission.

🧰 Common Troubleshooting

| Issue                                  | Fix                                                                 |
|---------------------------------------:|---------------------------------------------------------------------|
| fetch request blocked by CORS          | Ensure `$env:OLLAMA_ORIGINS="*"` is set before running `ollama serve`. |
| Could not connect to 0.0.0.0:11434     | Restart Ollama using `taskkill /F /IM ollama.exe /T` and rerun `ollama serve`. |
| Model not found                         | Recreate it using ollama create oneword -f Modelfile. Make sure your Modelfile and base model exist.   |
| Error: connection refused               | Make sure Ollama is still running in your terminal window.         |

✅ Example Output

```
Response: female
Full data: {
  "model": "oneword",
  "created_at": "...",
  "response": "female"
}
```

License / Attribution
- This README is provided as-is for instructional/demo purposes.
