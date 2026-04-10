# Focus Your Stream (FYS) - Chrome Extension

A browser extension that replaces YouTube's chat UI with a custom, unified multistream chat interface supporting YouTube, Twitch, polls, and keyword focus mode.

## Features

- **Multistream Chat**: Merge YouTube and Twitch chat into a single unified interface
- **Platform Labels**: Clear [YT]/[TW] tags identify message sources
- **Keyword Focus Mode**: Highlight important messages by filtering with keywords
- **Poll Detection**: Automatically detect and display YouTube/Twitch polls
- **Clean Dark Theme**: Professional UI with proper highlighting for different user roles
- **Auto-scroll**: Automatic scrolling to latest messages
- **OBS Ready**: Architecture prepared for OBS overlay integration

## Installation

### Step 1: Create Icons (Required for Chrome Web Store)

Create simple PNG icons in the `icons/` folder:
- `icon16.png` (16x16 pixels)
- `icon48.png` (48x48 pixels)
- `icon128.png` (128x128 pixels)

For development/testing, you can use any 16x16, 48x48, and 128x128 PNG images.

### Step 2: Load the Extension in Chrome

1. Open Chrome and navigate to `chrome://extensions/`
2. Enable "Developer mode" (toggle in top right)
3. Click "Load unpacked"
4. Select the `Focus Your Stream` folder

### Step 3: Usage

1. Navigate to a YouTube live stream or Twitch channel
2. The FYS UI will automatically replace/hide the default chat
3. Use the filter input to add keywords (comma-separated)
4. Matching messages will be highlighted
5. Open the extension popup for additional settings

## File Structure

```
Focus Your Stream/
├── manifest.json      # Extension configuration
├── background.js      # Central chat store and message broker
├── youtube.js         # YouTube content script
├── twitch.js          # Twitch content script
├── styles.css         # UI styling
├── popup.html         # Extension popup UI
├── popup.js           # Popup functionality
└── icons/             # Extension icons
    ├── icon16.png
    ├── icon48.png
    └── icon128.png
```

## Architecture

### Background Script (`background.js`)
- Central data store for all chat messages
- Manages settings persistence
- Handles message routing between content scripts
- Broadcasts updates to all connected tabs

### Content Scripts
- **youtube.js**: Injects into YouTube live streams
  - Waits for `#chat-messages` DOM element
  - Extracts messages using MutationObserver
  - Detects YouTube polls
  - Injects FYS UI

- **twitch.js**: Injects into Twitch channels
  - Waits for Twitch chat container
  - Extracts messages with badge information
  - Detects Twitch polls
  - Injects FYS UI

### Adding New Platforms

To add support for Kick, Discord, or other platforms:

1. Create a new content script (e.g., `kick.js`)
2. Follow the same pattern as `youtube.js`/`twitch.js`
3. Register in `manifest.json` under `content_scripts`
4. Update `background.js` to handle the new platform in `addMessage()` and `updatePoll()`

### OBS Integration (Future)

The architecture is designed for OBS overlay integration:

```javascript
// Access chat data from any page
chrome.runtime.sendMessage({ type: 'getState' }, (response) => {
  console.log(response.messages); // Latest merged messages
  console.log(response.polls);     // Active polls
});
```

## Configuration Options

- **Platform Toggles**: Enable/disable YouTube or Twitch
- **Keywords**: Comma-separated list for highlighting
- **Auto Scroll**: Toggle automatic scrolling
- **Timestamps**: Show/hide message timestamps
- **Keyword Mode**: Highlight only or filter-only mode

## Browser Compatibility

- Chrome 88+ (Manifest V3)
- Edge 88+ (Chromium-based)
- Brave (with Chrome extension support)

## Known Limitations

- Requires live chat to be visible on page
- May need page refresh when navigating between streams
- Poll detection depends on DOM structure

## Troubleshooting

**Chat not appearing:**
- Ensure you're on a live stream with chat enabled
- Check browser console for errors
- Try refreshing the page

**Keywords not highlighting:**
- Use comma-separated values without spaces
- Keywords are case-insensitive

**Extension not loading:**
- Verify all files are present
- Check `chrome://extensions/` for errors
- Ensure Developer mode is enabled

## License

MIT License - Free to use, modify, and distribute.
