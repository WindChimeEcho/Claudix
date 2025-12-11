<script setup lang="ts">
defineProps<{
  title: string
}>();

defineEmits<{
  (e: 'menuClick'): void;
  (e: 'newChat'): void;
}>();
</script>

<template>
  <div class="chat-header">
    <div class="header-left">
      <button class="menu-btn" @click="$emit('menuClick')">
        <span class="codicon codicon-menu"></span>
      </button>
      <h2 class="chat-title">{{ title }}</h2>
    </div>
    <div class="header-right">
      <button class="new-chat-btn" title="新开对话" @click="$emit('newChat')">
        <span class="codicon codicon-plus"></span>
      </button>
    </div>
  </div>
</template>

<style lang="scss">
.chat-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid var(--vscode-panel-border);
  min-height: 32px;
  padding: 0 12px;
}

.header-left {
  display: flex;
  align-items: center;
  gap: 8px;
  overflow: hidden;
  flex: 1;
}

.menu-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 24px;
  height: 24px;
  border: none;
  background: transparent;
  color: var(--vscode-titleBar-activeForeground);
  border-radius: 3px;
  cursor: pointer;
  transition: background-color 0.2s;
  opacity: 0.7;
}

.menu-btn .codicon {
  font-size: 12px;
}

.menu-btn:hover {
  background: var(--vscode-toolbar-hoverBackground);
  opacity: 1;
}

.chat-title {
  margin: 0;
  font-size: 12px;
  font-weight: 600;
  color: var(--vscode-titleBar-activeForeground);
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.header-right {
  display: flex;
  gap: 4px;
}

.new-chat-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 24px;
  height: 24px;
  border: none;
  background: transparent;
  color: var(--vscode-titleBar-activeForeground);
  border-radius: 3px;
  cursor: pointer;
  transition: background-color 0.2s;
  opacity: 0.7;
}

.new-chat-btn .codicon {
  font-size: 12px;
}

.new-chat-btn:hover {
  background: var(--vscode-toolbar-hoverBackground);
  opacity: 1;
}
</style>