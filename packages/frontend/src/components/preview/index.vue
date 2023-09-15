<template>
  <div class="iframe-container" ref="container"></div>
</template>

<script setup lang="ts">
import srcdoc from './srcdoc.html?raw'
import { PreviewProxy } from './PreviewProxy'
type Files = {
  [key: string]: string;
}

const props = withDefaults(
    defineProps<{
      files: Files
    }>(),
    {
    }
)

const container = ref()
const runtimeError = ref()

let sandbox: HTMLIFrameElement
let proxy: PreviewProxy
let stopUpdateWatcher: WatchStopHandle | undefined

// create sandbox on mount
onMounted(createSandbox)


function createSandbox() {
  if (sandbox) {
    // clear prev sandbox
    proxy.destroy()
    // stopUpdateWatcher && stopUpdateWatcher()
    container.value.removeChild(sandbox)
  }

  sandbox = document.createElement('iframe')
  sandbox.setAttribute(
    'sandbox',
    [
      'allow-forms',
      'allow-modals',
      'allow-pointer-lock',
      'allow-popups',
      'allow-same-origin',
      'allow-scripts',
      'allow-top-navigation-by-user-activation',
    ].join(' ')
  )

  const sandboxSrc = srcdoc.replace(/<!-- PREVIEW-OPTIONS-HEAD-HTML -->/, '')
//   previewOptions?.headHTML || ''

  sandbox.srcdoc = sandboxSrc
  container.value.appendChild(sandbox)

  proxy = new PreviewProxy(sandbox, {
    on_fetch_progress: (progress: any) => {
      // pending_imports = progress;
    },
    on_error: (event: any) => {
      const msg =
        event.value instanceof Error ? event.value.message : event.value
      if (
        msg.includes('Failed to resolve module specifier') ||
        msg.includes('Error resolving module specifier')
      ) {
        runtimeError.value =
          msg.replace(/\. Relative references must.*$/, '') +
          `.\nTip: edit the "Import Map" tab to specify import paths for dependencies.`
      } else {
        runtimeError.value = event.value
      }
    },
    on_unhandled_rejection: (event: any) => {
      let error = event.value
      if (typeof error === 'string') {
        error = { message: error }
      }
      runtimeError.value = 'Uncaught (in promise): ' + error.message
    },
    on_console: (log: any) => {
      if (log.duplicate) {
        return
      }
      if (log.level === 'error') {
        if (log.args[0] instanceof Error) {
          runtimeError.value = log.args[0].message
        } else {
          runtimeError.value = log.args[0]
        }
      } else if (log.level === 'warn') {
        if (log.args[0].toString().includes('[Vue warn]')) {
          runtimeWarning.value = log.args
            .join('')
            .replace(/\[Vue warn\]:/, '')
            .trim()
        }
      }
    },
    on_console_group: (action: any) => {
      // group_logs(action.label, false);
    },
    on_console_group_end: () => {
      // ungroup_logs();
    },
    on_console_group_collapsed: (action: any) => {
      // group_logs(action.label, true);
    },
  })

  sandbox.addEventListener('load', () => {
    // proxy.handle_links()
    // stopUpdateWatcher = watchEffect(updatePreview)
  })
}

const execute = async () => {
  const codeToEval = []

//   `setTimeout(()=> {
//         document.querySelectorAll('style[css]').forEach(el => el.remove())
//         document.head.insertAdjacentHTML('beforeend', window.__css__.map(s => \`<style css>\${s}</style>\`).join('\\n'))
//       }, 1)`

  for(let lang in props.files){
    if(lang === 'html') {
      codeToEval.push(`document.body.innerHTML = '${props.files[lang]}'`)
    }
  }
  await proxy.eval(codeToEval)
}

defineExpose({
  execute,
})
</script>

<style scoped>
.iframe-container,
.iframe-container :deep(iframe) {
  width: 100%;
  height: 100%;
  border: none;
  background-color: #fff;
}
</style>