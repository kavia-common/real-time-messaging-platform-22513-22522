# Ocean Professional Chat Frontend

This is a SvelteKit app implementing a modern chat UI with:
- Ocean Professional theme (blue primary, amber accents)
- Responsive layout: sidebar (chats), top bar, main chat area, composer
- REST and WebSocket integrations with mock fallbacks

Configure endpoints via environment:
- VITE_API_BASE: REST API base URL (optional, if omitted uses mock)
- VITE_WS_URL: WebSocket URL (optional, if omitted uses mock)

Scripts:
- npm run dev
- npm run build
- npm run preview
- npm run lint
- npm run test

Structure:
- src/lib/styles/theme.css: Theme variables/utilities
- src/lib/types.ts: Shared interfaces
- src/lib/api/*: REST client and endpoints (with mock fallbacks)
- src/lib/realtime/socket.ts: WebSocket client (with mock fallback)
- src/lib/stores/appStore.ts: Global app state and actions
- src/lib/components/*: UI components
- src/routes/ChatPage.svelte: Main chat page
- src/routes/+page.svelte: Routes to ChatPage

Ready to connect to your backend when endpoints are set.
