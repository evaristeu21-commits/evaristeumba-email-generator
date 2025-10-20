# evaristeumba-email-generator
“AI-powered email generator in Evariste Umba’s professional style.”
# Evariste Email Generator

A simple web app that generates emails in **Evariste Umba’s personal style** using the OpenAI API.

## 🔧 Setup
1. Clone this repo:
   ```bash
   git clone https://github.com/<yourusername>/evariste-email-generator.git

{
  "name": "evariste-email-generator",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  },
  "dependencies": {
    "next": "14.1.0",
    "react": "18.2.0",
    "react-dom": "18.2.0"
  }
}

/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
};

module.exports = nextConfig;

export default async function handler(req, res) {
  if (req.method !== "POST") return res.status(405).end();

  const { prompt, apiKey } = req.body;
  if (!apiKey) return res.status(400).json({ error: "Missing OpenAI API key." });

  // The full style prompt from ChatGPT (summarized Markdown)
  const stylePrompt = `
You are Evariste Umba’s Email Assistant, designed to compose professional, diplomatic, and detailed emails in his authentic writing style.

Follow this email structure:
1. Subject line — concise and descriptive.
2. Greeting — “Good day,” “Dear [Title + Name],” or “Hope this email finds you well.”
3. Opening — short context or purpose.
4. Body — detailed explanation in short paragraphs or numbered points.
5. Closing — appreciation or next step.
6. Signature block — consistent with Evariste’s style.

Tone: Warm, respectful, balanced formal-friendliness.
Common closings:
"Regards, Evariste Umba" or "Kind regards, Evariste Umba-Tsumbu"
Include contact info if appropriate.

Now, using this style, write two versions of the following email request:
${prompt}
`;

  try {
    const response = await fetch("https://api.openai.com/v1/chat/completions", {
      method: "POST",
      headers: {
        Authorization: `Bearer ${apiKey}`,
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        model: "gpt-4o-mini",
        messages: [
          { role: "system", content: "You are a skilled email writer." },
          { role: "user", content: stylePrompt },
        ],
        max_tokens: 1000,
      }),
    });

    const data = await response.json();
    const output = data?.choices?.[0]?.message?.content ?? "No response from model.";
    res.status(200).json({ output });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
}

import { useState } from "react";

export default function Home() {
  const [apiKey, setApiKey] = useState("");
  const [prompt, setPrompt] = useState("");
  const [result, setResult] = useState("");
  const [loading, setLoading] = useState(false);

  const handleGenerate = async () => {
    setLoading(true);
    setResult("");
    try {
      const res = await fetch("/api/generate", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ prompt, apiKey }),
      });
      const data = await res.json();
      setResult(data.output || JSON.stringify(data));
    } catch (err) {
      setResult("Error generating email: " + err.message);
    }
    setLoading(false);
  };

  return (
    <div style={{ maxWidth: 800, margin: "2rem auto", fontFamily: "Arial" }}>
      <h1>Evariste Email Generator</h1>
      <p>Generate emails in Evariste Umba’s professional and diplomatic style.</p>

      <label>🔑 OpenAI API Key</label>
      <input
        type="password"
        placeholder="Enter your OpenAI API key"
        value={apiKey}
        onChange={(e) => setApiKey(e.target.value)}
        style={{ width: "100%", marginBottom: 12 }}
      />

      <label>✉️ Email Request / Topic</label>
      <textarea
        rows="5"
        placeholder="e.g. Write an email to a dean proposing a joint event on justice reform."
        value={prompt}
        onChange={(e) => setPrompt(e.target.value)}
        style={{ width: "100%", marginBottom: 12 }}
      />

      <button onClick={handleGenerate} disabled={loading}>
        {loading ? "Generating..." : "Generate Email"}
      </button>

      <h3 style={{ marginTop: 24 }}>Generated Email:</h3>
      <pre
        style={{
          whiteSpace: "pre-wrap",
          background: "#f5f5f5",
          padding: 12,
          borderRadius: 8,
        }}
      >
        {result}
      </pre>
    </div>
  );
}

body {
  background-color: #f9fafb;
  color: #111827;
  margin: 0;
  padding: 0;
}
button {
  background-color: #2563eb;
  color: white;
  border: none;
  padding: 10px 16px;
  border-radius: 8px;
  cursor: pointer;
}
button:hover {
  background-color: #1d4ed8;
}
input, textarea {
  border: 1px solid #d1d5db;
  border-radius: 6px;
  padding: 8px;
  font-size: 14px;
}

npm install

npm run dev


