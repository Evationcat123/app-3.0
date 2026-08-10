# Fullscreen window + fixes patch

## Requested change
- The IME window (browser + keys) now expands all the way from the bottom
  of the screen to the very top, instead of only wrapping its content
  height. Implemented via `Window.setLayout(MATCH_PARENT, MATCH_PARENT)`
  in `onCreate()` plus a matching `MATCH_PARENT` height on the root view
  in `onCreateInputView()`. The browser area (which already used
  `layout_weight=1`) simply grows into the extra space, so the keys row
  stays the same size at the bottom.

## Bugs fixed
- **Missing fullscreen-mode guard**: custom IMEs with a rich input view
  (like this one's WebView) can be silently replaced by the system's
  plain "extract" edit box in landscape / short-height situations.
  Overrode `onEvaluateFullscreenMode()` to return `false` so WebBoard's
  own view is always what's shown.
- **Backspace had no repeat-on-hold**: every standard keyboard lets you
  hold backspace to keep deleting; WebBoard required one tap per
  character. Added a `Handler`-based repeat (450 ms initial delay, 60 ms
  interval) on the backspace key, with proper cleanup in
  `onFinishInputView()`/`onDestroy()` so nothing keeps firing after the
  keyboard closes.
