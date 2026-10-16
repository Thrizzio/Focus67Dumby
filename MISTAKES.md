# Known Pitfalls & Guardrails (DO NOT REPEAT)

1. **Blocking the Tkinter Mainloop:** Never run `requests.post()` to Ollama or `time.sleep()` on the main UI thread. The countdown timer will freeze for 200ms–1000ms every check.
2. **Polling Unchanged Windows:** Do not call Ollama if `current_window_title == last_evaluated_title`. Cache the result in a dict (`dict[str, dict]`) per session so sitting on VS Code for 30 minutes only triggers 1 LLM call total.
3. **Bloated Prompts for 270M/1B Models:** Do not use complex chain-of-thought prompts. Small Gemma models require concise, direct instructions and explicit JSON keys (`{"aligned": true, "nudge": "..."}`).
4. **Hardcoding Model Names:** Never hardcode a single model string. Always fetch available models from `http://localhost:11434/api/tags` and fallback gracefully if Ollama is offline.