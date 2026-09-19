# pi-plan-mode

A Pi extension that registers `/plan` to enter PLAN MODE with proper newline
preservation. Replaces the `plan.md` prompt-template approach, which mangled
`$ARGUMENTS` newlines.

## Install

```bash
pi install git:github.com/edisoncks/pi-plan-mode@v1.0.0
```

## Usage

```text
/plan Fix the login bug
/plan
  Fix the login bug
  Also make sure to add tests
```

`/plan <task>` (single line) dispatches through the registered command.
`/plan` followed by a newline/tab is intercepted via the `input` event so the
multi-line task is forwarded verbatim (newlines preserved).

## Activation contract

Plan mode is entered only when the input starts with `/plan`. Anything before
it (e.g. a pasted image path) is left untouched and the message is sent to the
model as-is.

## Known limitations

- `pi -p` / `--mode json`: `sendUserMessage()` is fire-and-forget and print
  mode disposes the session as soon as `prompt()` returns, so `/plan` is dropped.
- Queued input (steer/followUp, e.g. messages queued during compaction) does not
  emit the input event, so `/plan\n...` reaches the model raw.
- The command path cannot receive attached images, so RPC prompts that attach
  images to a single-line `/plan ...` lose them; the input path forwards them.
