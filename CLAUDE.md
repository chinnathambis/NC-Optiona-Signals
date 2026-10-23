# NC Option Signals — notes for working sessions

Owner's own recreation of an NC option-signals indicator for NIFTY (Pine v6). Current code `nc_option_signals.pine` (v0.10), older builds in `archive/`. Read `BUILD_QUEUE.md` and `RESEARCH.md` first. **Historical test 18 Oct 2026: FAIL** (`BACKTEST_2026-10-18.md`; PF 0.93 after costs). Private/personal script: never publish it under the original author's name. The owner wants to go step by step.

## How the owner wants to work
- Owner: Chinnathambi (IST, works from an iPhone). Plain-English explanations; no jargon without a line of meaning.
- **Build list first:** answer questions and add fixes/features to `BUILD_QUEUE.md` with a priority (P0, P1, …). Change code only when the owner says **build**, then build the whole list in one go.
- **Mock before building** visual changes; keep approved mocks and note why others were dropped. Dropped ideas stay in the docs, struck through with the reason.
- **Versions:** bump the version on every build; the previous file goes to `archive/` with its version in the name; log it.
- **Studies and back-tests never change the indicator by themselves:** report results, the owner decides.
- Pine can't be compiled here: the owner pastes the file into TradingView and reports errors. Say what still needs checking there.
- Read only the files a task needs. Nothing is kept on the owner's Mac: this repo is the only copy, so commit every build. Never commit keys or secrets.
- Docs moved here from the claude.ai Project on 23 Oct 2026; the repo is the master copy.
