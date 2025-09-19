<script lang="ts">
  import Layout from "$lib/components/Layout.svelte";
  import Sidebar from "$lib/components/Sidebar.svelte";
  import ChatHeader from "$lib/components/ChatHeader.svelte";
  import MessagesList from "$lib/components/MessagesList.svelte";
  import MessageComposer from "$lib/components/MessageComposer.svelte";
  import { appStore, initApp, selectedChat, selectedMessages } from "$lib/stores/appStore";
  import type { User, Chat, Message } from "$lib/types";
  import ActionButtons from "./ActionButtons.svelte";

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

  // Provide a render function compatible with {@render actions()} in Layout
  function actions() {
    // Return a component function; Layout will render it via {@render actions()}
    return ActionButtons;
  }
</script>

<Layout actions={actions}>
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
