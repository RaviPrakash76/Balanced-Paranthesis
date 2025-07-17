<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Balanced Parentheses Checker</title>
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(to right, #f5f7fa, #c3cfe2);
    }

    nav {
      background-color: #2c3e50;
      color: white;
      padding: 15px 30px;
      display: flex;
      justify-content: space-between;
    }

    nav a {
      color: white;
      text-decoration: none;
      margin-left: 20px;
      font-weight: 500;
      cursor: pointer;
    }

    .container {
      max-width: 800px;
      background: #fff;
      margin: 40px auto;
      padding: 30px;
      border-radius: 12px;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
    }

    h1 {
      color: #2c3e50;
      margin-bottom: 20px;
    }

    textarea {
      width: 100%;
      height: 120px;
      font-size: 16px;
      padding: 12px;
      border-radius: 8px;
      border: 2px solid #ddd;
      resize: none;
      transition: border 0.3s ease;
    }

    textarea:focus {
      border-color: #3498db;
      outline: none;
    }

    .buttons {
      display: flex;
      justify-content: space-between;
      margin-top: 20px;
    }

    button {
      padding: 10px 20px;
      font-size: 15px;
      background-color: #3498db;
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }

    button:hover {
      background-color: #2980b9;
    }

    .result {
      margin-top: 20px;
      font-size: 17px;
      font-weight: bold;
      padding: 12px;
      border-radius: 6px;
      display: none;
      text-align: center;
    }

    .valid {
      background-color: #e8f5e9;
      color: #2e7d32;
      border: 2px solid #66bb6a;
    }

    .invalid {
      background-color: #ffebee;
      color: #c62828;
      border: 2px solid #ef5350;
    }

    .char-count {
      text-align: right;
      margin-top: 5px;
      font-size: 13px;
      color: #666;
    }

    .footer {
      text-align: center;
      font-size: 12px;
      margin-top: 25px;
      color: #888;
    }

    .hidden {
      display: none;
    }

    p {
      font-size: 16px;
      line-height: 1.6;
      color: #444;
    }

    ul {
      padding-left: 20px;
    }

    code {
      background: #f0f0f0;
      padding: 2px 5px;
      border-radius: 4px;
    }
  </style>
</head>
<body>
  <nav>
    <div><strong>Parentheses Checker</strong></div>
    <div>
      <a onclick="showSection('home')">Home</a>
      <a onclick="showSection('about')">About</a>
    </div>
  </nav>

  <!-- Home Section -->
  <div class="container" id="home">
    <h1>Balanced Parentheses Checker</h1>
    <textarea id="expression" placeholder="Example: {[a+(b*c)] - d} / e"></textarea>
    <div class="char-count" id="charCount">0 characters</div>

    <div class="buttons">
      <button onclick="checkBalance()">Check Balance</button>
      <button onclick="clearInput()">Clear</button>
    </div>

    <div id="result" class="result"></div>

    <div class="footer">Made with ❤️ using HTML, CSS, and JavaScript</div>
  </div>

  <!-- About Section -->
  <div class="container hidden" id="about">
    <h1>About This Project</h1>
    <p>This project is a Balanced Parentheses Checker built using HTML, CSS, and JavaScript. It validates whether parentheses, brackets, and braces in an expression are used correctly.</p>

    <h2>Why Use a Stack?</h2>
    <p>The Stack data structure follows the Last-In-First-Out (LIFO) principle, which is perfect for matching open-close bracket pairs. When an opening bracket is found, it's pushed to the stack. When a closing bracket is found, it must match the top of the stack.</p>

    <h2>How the Algorithm Works</h2>
    <ul>
      <li>Loop through the expression character by character</li>
      <li>Push opening brackets to a stack</li>
      <li>On closing brackets, check if it matches the top of the stack</li>
      <li>If mismatched or stack is empty — expression is invalid</li>
      <li>If the stack is empty at the end — expression is balanced</li>
    </ul>

    <h2>Example</h2>
    <p>✅ Valid: <code>{ [ ( ) ] }</code><br>❌ Invalid: <code>{ ( [ ) ] }</code></p>

    <div class="footer">Educational project for stack-based validation 🧠</div>
  </div>

  <script>
    const input = document.getElementById("expression");
    const resultDiv = document.getElementById("result");
    const charCount = document.getElementById("charCount");

    input.addEventListener("input", () => {
      charCount.textContent = input.value.length + " characters";
    });

    function clearInput() {
      input.value = "";
      resultDiv.style.display = "none";
      charCount.textContent = "0 characters";
    }

    function checkBalance() {
      const str = input.value.trim();
      const stack = [];
      const pairs = { ')': '(', ']': '[', '}': '{' };
      const opening = ['(', '[', '{'];
      const closing = [')', ']', '}'];

      for (let i = 0; i < str.length; i++) {
        let char = str[i];
        if (opening.includes(char)) {
          stack.push({ char, index: i });
        } else if (closing.includes(char)) {
          if (stack.length === 0 || stack[stack.length - 1].char !== pairs[char]) {
            return showResult(false, `Mismatch at position ${i + 1} (found '${char}')`);
          }
          stack.pop();
        }
      }

      if (stack.length > 0) {
        const last = stack[stack.length - 1];
        return showResult(false, `Unmatched '${last.char}' at position ${last.index + 1}`);
      }

      showResult(true, "The expression is balanced ✅");
    }

    function showResult(success, message) {
      resultDiv.textContent = message;
      resultDiv.className = success ? "result valid" : "result invalid";
      resultDiv.style.display = "block";
    }

    function showSection(sectionId) {
      document.getElementById("home").classList.add("hidden");
      document.getElementById("about").classList.add("hidden");
      document.getElementById(sectionId).classList.remove("hidden");
    }
  </script>
</body>
</html>
