# 🚀 Chat Widget - Quick Start (2 Minutes)

## Option A: HuggingFace (Easiest)

1. Go to https://huggingface.co → Sign up (free)
2. Create an API key: Profile → Settings → Access Tokens → New Token (Read)
3. Open `index.html` and find this line (~line 1730):
   ```javascript
   apiKey: 'YOUR_HUGGING_FACE_API_KEY_HERE',
   ```
   Replace it with your real key:
   ```javascript
   apiKey: 'hf_xxxxxxxxxxxxxxxxxxxxx',
   ```
4. Save and reload your site - **Done!** ✅

**Limits:** 25,000 API calls/month (free tier)

---

## Option B: Ollama (Local, Zero Cost)

1. Download from https://ollama.ai and install
2. Run in terminal:
   ```bash
   ollama pull mistral
   ollama serve
   ```
3. Open `index.html` and change:
   ```javascript
   provider: 'oleama',  // Change from 'huggingface'
   ```
4. Keep Ollama running + reload site - **Done!** ✅

**Limits:** None (runs on your computer, needs 7GB+ disk space)

---

## That's It! 

Your chat widget is now live. Test it on your homepage!

For detailed setup docs, see [CHAT_SETUP.md](./CHAT_SETUP.md)
