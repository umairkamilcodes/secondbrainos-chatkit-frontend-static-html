# Second Brain OS ChatKit Frontend (Static HTML)

A single-file AI chat interface powered by [Second Brain OS](https://secondbrainos.com) and OpenAI's ChatKit. No build tools, no dependencies — just HTML.

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/umairkamilcodes/secondbrainos-chatkit-frontend-static-html)
[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/umairkamilcodes/secondbrainos-chatkit-frontend-static-html)
[![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy?repo=https://github.com/umairkamilcodes/secondbrainos-chatkit-frontend-static-html)

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

Use one of the deploy buttons above, or upload `index.html` to any static hosting:

- **Vercel** - Click the deploy button above (recommended)
- **Netlify** - Click the deploy button above
- **Render** - Click the deploy button above
- **Cloudflare Pages** - Connect your GitHub repo directly in the Cloudflare dashboard
- **GitHub Pages** - Push to a `gh-pages` branch
- **S3 + CloudFront** - Upload to a bucket with static hosting enabled
- **Any web server** - Just serve the HTML file

## Embed in Any Website

You can add the ChatKit widget to any existing website — WordPress, Squarespace, Wix, or any custom HTML page. Just add this code snippet:

### Option 1: Inline Chat Widget

Add the ChatKit script to your `<head>`:

```html
<head>
  <script src="https://cdn.platform.openai.com/deployments/chatkit/chatkit.js" async></script>
</head>
```

Then add this where you want the chat to appear in your `<body>`:

```html
<!-- ChatKit Container -->
<openai-chatkit id="my-chat" style="width: 100%; max-width: 800px; height: 600px;"></openai-chatkit>

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
        theme: { colorScheme: 'light' },
      });
    } else {
      setTimeout(initChatKit, 50);
    }
  }

  initChatKit();
</script>
```

### Option 2: WordPress

1. Create a new page or use a **Custom HTML** block
2. Paste the full code snippet from Option 1

### Option 3: Squarespace / Wix / Webflow

1. Add a **Custom Code** or **Embed** block to your page
2. Paste the full code snippet from Option 1
3. Adjust the container dimensions to fit your layout

### Styling the Widget

Customize the container to match your site:

```css
#my-chat {
  width: 100%;
  max-width: 600px;
  height: 500px;
  margin: 0 auto;
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0,0,0,0.1);
}
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

### Widget Mode (Floating Toggle)

ChatKit doesn't have a native widget mode, but you can create one with a custom wrapper. Add this CSS and HTML for a floating chat button:

```html
<style>
  .chat-widget-toggle {
    position: fixed;
    bottom: 24px;
    right: 24px;
    width: 60px;
    height: 60px;
    border-radius: 50%;
    background: #2563eb;
    border: none;
    cursor: pointer;
    box-shadow: 0 4px 12px rgba(37, 99, 235, 0.4);
    z-index: 1000;
  }
  .chat-widget-toggle svg {
    width: 28px;
    height: 28px;
    fill: white;
  }
  .chat-widget-toggle.open .icon-chat { display: none; }
  .chat-widget-toggle.open .icon-close { display: block; }
  .chat-widget-toggle .icon-close { display: none; }

  .chat-widget-container {
    position: fixed;
    bottom: 100px;
    right: 24px;
    width: 400px;
    height: 600px;
    border-radius: 16px;
    overflow: hidden;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.15);
    opacity: 0;
    visibility: hidden;
    transform: translateY(20px) scale(0.95);
    transition: opacity 0.3s, transform 0.3s, visibility 0.3s;
    z-index: 999;
  }
  .chat-widget-container.open {
    opacity: 1;
    visibility: visible;
    transform: translateY(0) scale(1);
  }
</style>

<button class="chat-widget-toggle" id="widget-toggle">
  <svg class="icon-chat" viewBox="0 0 24 24"><path d="M20 2H4c-1.1 0-2 .9-2 2v18l4-4h14c1.1 0 2-.9 2-2V4c0-1.1-.9-2-2-2zm0 14H6l-2 2V4h16v12z"/></svg>
  <svg class="icon-close" viewBox="0 0 24 24"><path d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12z"/></svg>
</button>

<div class="chat-widget-container" id="widget-container">
  <openai-chatkit id="widget-chat" style="width:100%;height:100%;"></openai-chatkit>
</div>

<script>
  const toggle = document.getElementById('widget-toggle');
  const container = document.getElementById('widget-container');
  toggle.addEventListener('click', () => {
    toggle.classList.toggle('open');
    container.classList.toggle('open');
  });
</script>
```

See `chatkit-widget-mode.html` for a complete working example.

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
