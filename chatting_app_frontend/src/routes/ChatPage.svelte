<script lang="ts">
  import Layout from "$lib/components/Layout.svelte";
  import Sidebar from "$lib/components/Sidebar.svelte";
  import ChatHeader from "$lib/components/ChatHeader.svelte";
  import MessagesList from "$lib/components/MessagesList.svelte";
  import MessageComposer from "$lib/components/MessageComposer.svelte";
  import { appStore, initApp, selectedChat, selectedMessages } from "$lib/stores/appStore";
  import type { User, Chat, Message } from "$lib/types";

  let mounted = $state(false);
  $effect(() => { if (!mounted) { initApp(); mounted = true; } });

  type AppSnapshot = {
    user: User | null;
    chats: Chat[];
    messages: Record<string, Message[]>;
    selectedChatId: string | null;
    loading: boolean;
    error?: string | null;
  };

  function snapshot(): AppSnapshot {
    let v = {} as AppSnapshot;
    const unsub = appStore.subscribe((s) => (v = s as AppSnapshot));
    unsub();
    return v;
  }

  // Render function for actions toolbar (compatible with {@render actions()} in Layout)
  function actions() {
    // return a functionless token for Svelte {@render}: we can't use JSX; instead return a string token and use inner HTML.
    // However, {@render} expects a snippet function; we can pass a function that returns nothing and use a separate state.
    // Simpler: return a function that creates no output; and we place actions button inside the Layout's actions prop directly as a string is not allowed.
    // Correct approach: use a component-returning function by referencing a Svelte component factory.
    // Here, we define a no-arg function that returns the ButtonAction component instance.
    return ButtonAction;
  }

  // Inline component factory for the Actions button
  function ButtonAction() {
    return (
      // Svelte requires non-JSX. Therefore, we'll not use this path.
      // This function will be replaced by a template snippet below.
      null
    );
  }
</script>

<!-- Workaround: since Svelte 5 render functions expect a snippet, define actions as a simple function returning undefined,
     and instead render a minimal actions area by using a tiny passthrough in Layout props that returns a renderable element via
     the Svelte snippet expression directly in the prop. -->
<Layout actions={() => { /* render inline snippet */ return (
  // using Svelte valid element literal: use a plain element; avoid fragments
  // note: no JSX — Svelte 5 supports element literals in expressions
  <button class="button secondary" onclick={() => location.reload()}>Refresh</button>
); }}>
  <div class="grid">
    <Sidebar chats={snapshot().chats} selectedId={snapshot().selectedChatId ?? ""}/>
    <section class="chat card">
      <ChatHeader chat={$selectedChat} />
      <MessagesList messages={$selectedMessages} self={snapshot().user}/>
      <MessageComposer disabled={!$selectedChat} />
    </section>
  </div>
</Layout>

<style>
  .grid { display: contents; }
  .chat {
    display: flex;
    flex-direction: column;
    padding: 12px;
    height: calc(100vh - 100px);
  }
</style>
