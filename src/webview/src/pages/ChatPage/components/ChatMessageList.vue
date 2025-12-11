<script setup lang="ts">
import { ref } from 'vue';
import ClaudeWordmark from '@/components/ClaudeWordmark.vue';
import RandomTip from '@/components/RandomTip.vue';
import MessageRenderer from '@/components/Messages/MessageRenderer.vue';
import Spinner from '@/components/Messages/WaitingIndicator.vue';
import type { ToolContext } from '@/types/tool';
import type { PermissionMode } from '@anthropic-ai/claude-agent-sdk';

// Props
interface Props {
  messages: any[];
  isBusy: boolean;
  permissionRequestsLen: number;
  platform: string;
  toolContext: ToolContext;
  permissionMode: PermissionMode;
}

const props = defineProps<Props>();

// Refs
const endEl = ref<HTMLDivElement | null>(null);

// 暴露滚动方法供父组件调用
function scrollToBottom(): void {
  const end = endEl.value;
  if (!end) return;
  requestAnimationFrame(() => {
    try {
      end.scrollIntoView({ block: 'end' });
    } catch { }
  });
}

defineExpose({
  scrollToBottom
});
</script>

<template>
  <div ref="containerEl"
    :class="['messagesContainer', 'custom-scroll-container', { dimmed: permissionRequestsLen > 0 }]">
    <template v-if="messages.length === 0">
      <div v-if="isBusy" class="emptyState">
        <div class="emptyWordmark">
          <ClaudeWordmark class="emptyWordmarkSvg" />
        </div>
      </div>
      <div v-else class="emptyState">
        <div class="emptyWordmark">
          <ClaudeWordmark class="emptyWordmarkSvg" />
        </div>
        <RandomTip :platform="platform" />
      </div>
    </template>
    <template v-else>
      <MessageRenderer v-for="(m, i) in messages" :key="m?.id ?? i" :message="m" :context="toolContext" />
      <div v-if="isBusy" class="spinnerRow">
        <Spinner :size="16" :permission-mode="permissionMode" />
      </div>
      <div ref="endEl" />
    </template>
  </div>
</template>

<style scoped>
.messagesContainer {
  flex: 1;
  overflow-y: auto;
  overflow-x: hidden;
  padding: 8px 0 12px;
  position: relative;
}

.messagesContainer.dimmed {
  filter: blur(1px);
  opacity: 0.5;
  pointer-events: none;
}

.msg-list {
  display: flex;
  flex-direction: column;
  gap: 8px;
  padding: 0 12px;
}

.msg-item {
  background: var(--vscode-editor-background);
  border: 1px solid var(--vscode-panel-border);
  border-radius: 6px;
  padding: 8px;
}

.json-block {
  margin: 0;
  white-space: pre-wrap;
  word-break: break-word;
  font-family: var(--app-monospace-font-family,
      ui-monospace,
      SFMono-Regular,
      Menlo,
      Monaco,
      Consolas,
      'Liberation Mono',
      'Courier New',
      monospace);
  font-size: var(--app-monospace-font-size, 12px);
  line-height: 1.5;
  color: var(--vscode-editor-foreground);
}
</style>