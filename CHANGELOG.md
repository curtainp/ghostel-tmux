# Changelog

All notable changes to this project will be documented in this file.

## [Unreleased]

### Added
- Functions for packages that build on ghostel-tmux, see "For other
  packages" in the README: `ghostel-tmux-controller`,
  `ghostel-tmux-controllers`, `ghostel-tmux-session-name`,
  `ghostel-tmux-session-id`, `ghostel-tmux-panes`,
  `ghostel-tmux-pane-buffer`, `ghostel-tmux-active-pane`,
  `ghostel-tmux-pane-id`, `ghostel-tmux-window-id`,
  `ghostel-tmux-window-split`, `ghostel-tmux-select-pane`, and
  `ghostel-tmux-command`, which sends tmux a command and gives a callback
  its reply.
- `ghostel-tmux-attach-hook` runs when the panes of a session are there,
  `ghostel-tmux-detach-hook` when its tmux client ends, and
  `ghostel-tmux-notification-functions` for each notification of tmux.
- `ghostel-tmux-attach` and `ghostel-tmux-new` take NO-DISPLAY: no window
  is to show the session, and the echo area does not report the attach.
  `ghostel-tmux-detach` takes the BUFFER whose session to detach.

### Changed
- `ghostel-tmux-attach` and `ghostel-tmux-new` show a session that one
  of them is attached to already in the selected window and return its
  controller, where they started a second tmux client.
- "Attached to SESSION" is said only while the echo area still says
  "Attaching to SESSION...": an attach called with `inhibit-message`
  says neither.
- `ghostel-tmux-pane-mode-hook` runs in a pane buffer that knows its
  session, tmux window and pane.

### Fixed
- A pane that arrives while a minibuffer is open, as after
  `ghostel-tmux-attach`, keeps its window when the minibuffer closes.
  A split, a switch to another pane or tmux window, and a pane or tmux
  window that ended, all reported by tmux meanwhile, are shown once it
  has closed.
- A takeover whose tmux leaves while a minibuffer is open shows the
  shell again once it has closed.

## [0.3.0] — 2026-11-14

### Added
- A password prompt in a pane is read with `read-passwd`, as in a
  ghostel buffer, when `ghostel-tmux-attach` or `ghostel-tmux-new` runs
  tmux on this machine.  A pane started again with `respawn-pane` gets
  this back when the session is attached again.
- `ghostel-tmux-socket` selects a tmux server other than the default
  one, by socket name or path; with a prefix argument,
  `ghostel-tmux-attach`, `ghostel-tmux-new` and `ghostel-tmux-capture`
  prompt for it.  It and `ghostel-tmux-program` can be set per host
  with connection-local variables.
- `ghostel-tmux-detach` with a prefix argument keeps the session's pane
  buffers, whatever `ghostel-tmux-keep-detached-panes` says.

### Fixed
- An Emacs input method such as `chinese-py` composes in a pane.
- `C-t o`, `C-t C-n`, `C-t C-p` and `C-t s` select the pane in Emacs at
  once: keys typed before tmux reported the switch went to the pane left.
  In a zoomed split, the pane is still shown once tmux has unzoomed.
- Keys typed while a command is in flight join one `send-keys`, so
  typing no longer lags one round trip per key over a slow link.
- With `ghostel-tmux-attach` and `ghostel-tmux-new` on macOS, pane
  output no longer falls behind a program that prints without pause.
- A remote tmux started through TRAMP's direct async processes works:
  it runs after `stty raw -echo`.

### Changed
- Ghostel 0.65.0 or later is required.
- `C-t o` cycles the tiled panes of the window from the one shown, so it
  no longer reaches floating panes (tmux 3.8).
- `ghostel-tmux-attach` and `ghostel-tmux-new` return at once instead
  of waiting for the first pane; the selected window shows tmux's
  active pane when it arrives.
- Attaching again to a session whose client was killed or whose link
  dropped makes its sealed pane buffers live again, in the windows that
  show them, instead of creating new buffers beside them.
- Pane output is decoded faster, which matters most when a program
  floods its pane.

## [0.2.0] — 2026-11-11

### Added
- `ghostel-tmux-new` and `ghostel-tmux-attach` connect to a tmux session
  in control mode (`tmux -CC`) and show each pane as a ghostel buffer,
  backfilled with its scrollback and terminal state.  The panes of a tmux
  window are Emacs window splits, and the tmux windows are tabs in the
  tab line; with `ghostel-tmux-split-windows` nil, a window shows one
  zoomed pane.
- `ghostel-tmux-takeover-mode` turns a `tmux -CC` typed in a ghostel
  shell, also inside ssh, into panes, with the shell's terminal as the
  transport.
- Pane commands under the tmux prefix (`C-t`): next and previous pane,
  a pane picker, `ghostel-tmux-restore-split`, detach, and `C-t :` for
  tmux commands, whose answer is shown.
- `ghostel-tmux-capture` dumps a pane's scrollback into a buffer
  without a connection.
- With a remote `default-directory`, tmux runs on that host through
  TRAMP.
- `ghostel-tmux-reset` recovers from a wedged protocol stream.
- Shell wrappers in `shell/` that add `-CC` to `tmux`.

### Changed
- Requires ghostel 0.64.0, and uses its public functions only.

## [0.1.0] — 2026-11-06

First release.
