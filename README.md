# Second Brain OS ChatKit Frontend (Static HTML)

A single-file AI chat interface powered by [Second Brain OS](https://secondbrainos.com) and OpenAI's ChatKit. No build tools, no dependencies — just HTML.

[![Deploy to Cloudflare Pages](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/umairkamilcodes/secondbrainos-chatkit-frontend-static-html)

## Features

- **Zero Build Step** - Pure HTML/CSS/JS, works anywhere
- **OpenAI ChatKit** - Beautiful, responsive chat UI with file upload support
- **Domain Key Auth** - No API keys exposed in client code
- **Self-Hostable** - Deploy to any static hosting (GitHub Pages, Netlify, Cloudflare Pages, S3, etc.)

## Quick Start

### Step 1: Get Your OpenAI API Key

1. Go to [OpenAI Platform API Keys](https://platform.openai.com/api-keys)
2. Create a new API key or use an existing one
3. Save it securely — you'll need it for Step 3

### Step 2: Add Your Domain to OpenAI Allowlist

You must add your domain to OpenAI's domain allowlist for ChatKit to work:

1. Go to [OpenAI Platform Domain Allowlist](https://platform.openai.com/settings/organization/security/domain-allowlist)
2. Add your domain (e.g., `example.com`)
3. This **must match** the domain you'll configure in Second Brain OS

### Step 3: Configure Second Brain OS

In your [Second Brain OS profile](https://secondbrainos.com), configure these settings:

| Setting | Description |
|---------|-------------|
| `open_ai_api_key` | Your OpenAI API key (from Step 1) |
| `domain` | Your website domain (e.g., `example.com`) — must match Step 2 |
| `public_domain_key` | Your domain key (e.g., `domain_pk_xxx`) |
| `auto_responder_instructions` | System prompt for your AI assistant |
| `auto_responder_large_language_model` | Model to use (e.g., gpt-4o) |

### Step 4: Update index.html

Replace the placeholder domain key with your own:

```javascript
const DOMAIN_KEY = 'domain_pk_your_key_here';
```

### Step 5: Deploy

Upload `index.html` to any static hosting:

- **Cloudflare Pages** - Click the deploy button above, or connect your repo
- **GitHub Pages** - Push to a `gh-pages` branch
- **Netlify** - Drag and drop the file
- **Vercel** - Deploy as a static site
- **S3 + CloudFront** - Upload to a bucket with static hosting enabled
- **Any web server** - Just serve the HTML file

## Embed in Any Website

You can add the ChatKit widget to any existing website — WordPress, Squarespace, Wix, or any custom HTML page. Just add this code snippet:

### Option 1: Inline Chat Widget

Add this anywhere in your HTML where you want the chat to appear:

```html
<!-- ChatKit Container -->
<div id="my-chat" style="width: 100%; max-width: 800px; height: 600px;"></div>

<!-- ChatKit Script -->
<script src="https://cdn.platform.openai.com/deployments/chatkit/chatkit.js" async></script>
<script>
  const DOMAIN_KEY = 'domain_pk_your_key_here'; // Replace with your domain key
  const API_URL = 'https://openai.secondbrainos.com/chatkit';

  const customFetch = (url, options = {}) => {
    return fetch(url, {
      ...options,
      headers: {
        ...options.headers,
        'X-Domain-Key': DOMAIN_KEY,
      },
    });
  };

  function initChatKit() {
    const chatkit = document.getElementById('my-chat');
    if (chatkit && chatkit.setOptions) {
      chatkit.setOptions({
        api: {
          url: API_URL,
          domainKey: DOMAIN_KEY,
          fetch: customFetch,
        },
        theme: 'light',
      });
    } else {
      setTimeout(initChatKit, 50);
    }
  }

  initChatKit();
</script>
```

### Option 2: WordPress

1. Install a plugin like **Insert Headers and Footers** or **WPCode**
2. Add the ChatKit script to your footer:
   ```html
   <script src="https://cdn.platform.openai.com/deployments/chatkit/chatkit.js" async></script>
   ```
3. Create a new page or use a Custom HTML block, and paste the container + initialization script

### Option 3: Squarespace / Wix / Webflow

1. Add a **Custom Code** or **Embed** block to your page
2. Paste the full code snippet from Option 1
3. Adjust the container dimensions to fit your layout

### Styling the Widget

Customize the container to match your site:

```html
<style>
  #my-chat {
    width: 100%;
    max-width: 600px;
    height: 500px;
    margin: 0 auto;
    border-radius: 12px;
    box-shadow: 0 4px 20px rgba(0,0,0,0.1);
  }
</style>
```

## Security

- **No API keys in client code** - The domain key is public but only works from your domain
- **Origin validation** - Backend checks the `Origin` header matches the registered domain
- **Rate limiting** - Applied at the backend level per user

## Theming & Customization

All theming is done via the `setOptions()` call in your code. Here's a complete example with all available options:

```javascript
chatkit.setOptions({
  api: {
    url: API_URL,
    domainKey: DOMAIN_KEY,
    fetch: customFetch,
  },

  // Theme configuration
  theme: {
    colorScheme: 'light', // or 'dark'
    color: {
      accent: {
        primary: '#2563eb', // Your brand color
        level: 2,           // Intensity (1-3)
      },
    },
    radius: 'round',      // or 'sharp'
    density: 'spacious',  // or 'compact'
    typography: {
      fontFamily: "'Inter', sans-serif",
    },
  },

  // Start screen with greeting and suggested prompts
  startScreen: {
    greeting: 'How can I help you?',
    prompts: [
      {
        label: 'Get started',
        prompt: 'Help me get started',
        icon: 'search',
      },
      {
        label: 'Learn more',
        prompt: 'Tell me about your features',
        icon: 'info',
      },
    ],
  },

  // Input composer customization
  composer: {
    placeholder: 'Type your message...',
  },

  // Header customization
  header: {
    enabled: true,
    customButtonLeft: {
      icon: 'settings-cog',
      onClick: () => { console.log('Settings clicked'); },
    },
    customButtonRight: {
      icon: 'home',
      onClick: () => { window.location.href = '/'; },
    },
  },

  // Thread history sidebar
  history: {
    enabled: true, // Set false to hide thread history
  },
});
```

### Theme Options Reference

| Option | Values | Description |
|--------|--------|-------------|
| `theme.colorScheme` | `'light'`, `'dark'` | Light or dark mode |
| `theme.color.accent.primary` | Hex color | Primary accent color |
| `theme.color.accent.level` | `1`, `2`, `3` | Color intensity |
| `theme.radius` | `'round'`, `'sharp'` | Border radius style |
| `theme.density` | `'spacious'`, `'compact'` | UI density |
| `theme.typography.fontFamily` | CSS font-family | Custom font |

### Start Screen Prompts

Prompts appear as clickable buttons on the start screen:

```javascript
startScreen: {
  greeting: 'Welcome! What can I help with?',
  prompts: [
    { label: 'Button text', prompt: 'Message to send', icon: 'search' },
  ],
}
```

**Note**: Use `label` (not `name`) for the button text.

### Container Styling

Control the chat widget size via CSS:

```css
#my-chat {
  width: 100%;
  max-width: 800px;
  height: 600px;
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0,0,0,0.1);
}
```

## Troubleshooting

### "Invalid domain key" error

- Verify your domain key is correct
- Ensure your domain is registered with Second Brain OS

### "Domain key not authorized for origin" error

- The request is coming from a domain not registered for this key
- Check that you're accessing from the correct domain (not localhost, unless registered)

### Chat doesn't load

- Open browser console for errors
- Verify the ChatKit SDK script loaded successfully
- Check that `setOptions` was called (look for the polling `init()` function)

## Comparison with Next.js Version

| Feature | Static HTML | Next.js |
|---------|-------------|---------|
| Build step | None | Required |
| Deployment | Any static host | Vercel/Node.js host |
| API key handling | Domain key (public) | Server-side (hidden) |
| Customization | Edit HTML directly | React components |
| Best for | Simple deploys, WordPress, CMS | Full-stack apps |

## License

MIT
