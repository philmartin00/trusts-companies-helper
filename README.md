# trusts-companies-helper
A guidance tool to assist those needing help with trusts and companies 
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Trusts & Companies Helper</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 40px; }
    #chat { max-width: 600px; margin: auto; }
    .message { margin: 10px 0; }
    .user { color: blue; }
    .bot { color: green; }
    #input { width: 100%; padding: 10px; margin-top: 10px; }
  </style>
</head>
<body>
  <div id="chat">
    <h2>Trusts & Companies Helper</h2>
    <p><i>This tool offers general information only. Not legal advice.</i></p>
    <div id="messages"></div>
    <input type="text" id="input" placeholder="Ask your question..." />
  </div>

  <script>
    const input = document.getElementById('input');
    const messages = document.getElementById('messages');

    input.addEventListener('keypress', function(e) {
      if (e.key === 'Enter') {
        const question = input.value;
        addMessage('user', question);
        input.value = '';
        fetchAnswer(question);
      }
    });

    function addMessage(sender, text) {
      const div = document.createElement('div');
      div.className = 'message ' + sender;
      div.textContent = (sender === 'user' ? 'You: ' : 'Helper: ') + text;
      messages.appendChild(div);
    }

    async function fetchAnswer(question) {
      addMessage('bot', 'Thinking...');
      // Replace with your OpenAI endpoint or custom logic
      const response = await fetch('https://api.openai.com/v1/chat/completions', {
        method: 'POST',
        headers: {
          'Authorization': 'Bearer YOUR_OPENAI_API_KEY',
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          model: "gpt-4",
          messages: [{ role: "system", content: "You are a helpful assistant providing general guidance on trusts and companies, but not legal advice." }, { role: "user", content: question }]
        })
      });
      const data = await response.json();
      const answer = data.choices[0].message.content.trim();
      messages.lastChild.remove(); // remove "Thinking..."
      addMessage('bot', answer);
    }
  </script>
</body>
</html>
