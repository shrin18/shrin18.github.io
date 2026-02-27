# Chat Widget Setup Guide

Your portfolio now includes an AI-powered "Ask Me Anything" chat widget! Here's how to set it up.

## Quick Start Options

You have two main options:
1. **HuggingFace Inference API** (Recommended for beginners - free tier available)
2. **Ollama** (Fully local, no API keys, no rate limits)

---

## Option 1: HuggingFace Inference API (Recommended)

### Why Choose This?
- ✅ No setup required (just get a free API key)
- ✅ Works immediately on your live website
- ✅ Free tier available (25,000 API calls/month)
- ✅ No need to keep a server running

### Setup Steps

1. **Create a HuggingFace Account**
   - Go to https://huggingface.co
   - Sign up (free)
   - Go to your Profile → Settings → Access Tokens
   - Create a new "read" token

2. **Add Your API Key**
   - Open `index.html`
   - Find the line: `apiKey: 'YOUR_HUGGING_FACE_API_KEY_HERE',`
   - Replace it with: `apiKey: '[your_actual_token_here]',`
   - Save the file

3. **That's it!** 🎉
   - Your chat widget will now work on your live site
   - Users can ask questions and get AI responses

### Free Tier Limits
- 25,000 calls per month
- If you reach the limit, responses will be queued

### Available Models
The current setup uses `HuggingFaceH4/zephyr-7b-beta`. You can try other models:
- `mistralai/Mistral-7B-Instruct-v0.1` (faster, more lightweight)
- `meta-llama/Llama-2-7b-chat-hf` (requires approval)
- `NousResearch/Nous-Hermes-2-Mistral-7B-DPO`

Just change the model name in the `AI_CONFIG` object.

---

## Option 2: Ollama - Fully Local Solution

### Why Choose This?
- ✅ Completely free, no API keys needed
- ✅ Works completely offline (after download)
- ✅ No rate limits or quotas
- ✅ Privacy-focused (data stays on your computer)
- ❌ Only works when your computer is running (not ideal for live website)

### Setup Steps

1. **Install Ollama**
   - Download from https://ollama.ai
   - Install for macOS/Linux/Windows

2. **Download a Model**
   ```bash
   ollama pull mistral
   ```
   Other options:
   - `ollama pull neural-chat` (smaller, faster)
   - `ollama pull orca-mini` (very small, good for CPU)
   - `ollama pull openchat` (good balance)

3. **Start Ollama**
   ```bash
   ollama serve
   ```
   This runs the server on `http://localhost:11434`

4. **Configure Your Website**
   - Open `index.html`
   - Find: `provider: 'huggingface',`
   - Change to: `provider: 'ollama',`
   - Change the model if needed: `model: 'mistral',`

5. **Test It**
   - Keep Ollama running
   - Open your website locally (or on your network)
   - Chat widget should work!

### Performance Tips
- **Recommended models for CPU-only:**
  - `neural-chat` (~4GB)
  - `orca-mini` (~2GB)
  - `tinyllama` (~1.2GB)

- **Better models (need GPU):**
  - `mixtral` (70B, GPU recommended)
  - `mistral` (7B, works on CPU but slow)

---

## Advanced: Use Both (Fallback Setup)

Want the best of both worlds? You can set it up to use HuggingFace by default and fall back to Ollama locally:

```javascript
// In index.html, modify getChatResponse function:
async function getChatResponse(userMessage) {
    try {
        // Try local Ollama first
        if (await isOllamaAvailable()) {
            return await getOllamaResponse(userMessage);
        }
    } catch (error) {
        console.log('Ollama not available, trying HuggingFace...');
    }
    
    // Fall back to HuggingFace
    return await getHuggingFaceResponse(userMessage);
}

async function isOllamaAvailable() {
    try {
        const response = await fetch('http://localhost:11434/api/tags', {timeout: 2000});
        return response.ok;
    } catch {
        return false;
    }
}
```

---

## Troubleshooting

### Chat shows "I'm currently not connected to an AI service"
- You haven't set up an API key yet
- Check if you replaced `YOUR_HUGGING_FACE_API_KEY_HERE` with your actual key

### HuggingFace responses are slow
- Free tier has queuing during peak hours
- Consider upgrading or upgrading or using Ollama locally

### Ollama not working
- Make sure Ollama is running: `ollama serve`
- Confirm the model is downloaded: `ollama list`
- Check that the BaseURL is correct: `http://localhost:11434`

### CORS errors in console
- Ollama: Works locally (port 11434 is usually allowed)
- HuggingFace: Uses proper CORS headers, should work fine

---

## System Prompt Customization

The chat has a system prompt that tells the AI about you. You can customize it:

In `index.html`, find:
```javascript
const systemPrompt = `You are a helpful AI assistant representing Shrinish Donde's portfolio...`
```

Edit this to include more details about yourself:
- Your projects
- Your skills
- Your interests
- Specific information you want to share

---

## Security Considerations

### API Keys
- **HuggingFace API keys**: Store in environment variables for production
- **For GitHub/public repos**: Never commit your API key directly
- **Better approach**: Use a backend proxy server to hide your API key

Example with backend (Node.js):
```javascript
// Client-side
const response = await fetch('/api/chat', {
    method: 'POST',
    body: JSON.stringify({message: userMessage})
});

// Server-side (Node.js)
app.post('/api/chat', async (req, res) => {
    const huggingFaceResponse = await fetch('...', {
        headers: {Authorization: `Bearer ${process.env.HF_API_KEY}`}
    });
    res.json(await huggingFaceResponse.json());
});
```

### Privacy
- Messages are sent to the API provider (HuggingFace)
- If you use Ollama locally, no data leaves your computer
- Consider user privacy when collecting chat data

---

## Next Steps

1. Choose your setup option (HuggingFace or Ollama)
2. Follow the configuration steps
3. Test the chat widget on your site
4. Customize the system prompt with more about yourself
5. Deploy and share!

---

## Resources

- **HuggingFace**: https://huggingface.co/docs/api-inference
- **Ollama**: https://ollama.ai
- **Model Leaderboard**: https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard
- **Open Source Models**: https://github.com/taishi-i/awesome-ChatGPT-repositories

Enjoy your new chat widget! 🚀
