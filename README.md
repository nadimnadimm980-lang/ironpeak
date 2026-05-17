# ironpeak
{
  "rewrites": [
    { "source": "/api/chat", "destination": "/api/chat" },
    { "source": "/(.*)", "destination": "/public/index.html" }
  ]
}
export default async function handler(req, res) {
  if (req.method !== 'POST') return res.status(405).json({ error: 'Method not allowed' });

  const { system, messages } = req.body;

  try {
    const response = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'x-api-key': process.env.ANTHROPIC_API_KEY,
        'anthropic-version': '2023-06-01'
      },
      body: JSON.stringify({
        model: 'claude-sonnet-4-20250514',
        max_tokens: 1000,
        system,
        messages
      })
    });

    const data = await response.json();
    res.status(200).json(data);
  } catch (err) {
  // Vervang de fetch in de sendMessage functie met dit:
const res = await fetch('/api/chat', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    system: p.systemPrompt,
    messages: chatHistory
  })
});
const data = await res.json();
const reply = data.content?.find(b => b.type === 'text')?.text || 'No response received.';
hideTyping();
appendMessage('ai', reply);
chatHistory.push({ role: 'assistant', content: reply });
    res.status(500).json({ error: err.message });
  }
}
