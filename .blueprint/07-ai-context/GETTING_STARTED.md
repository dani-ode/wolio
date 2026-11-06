`.blueprint/07-ai-context/GETTING_STARTED.md`
# Ubuntu and OpenCode quickstart

## Before code

Review DOCUMENT_AUTHORITY, PROJECT_COVERAGE and revision 1.2. Resolve remaining
owner feedback, commit the reviewed documentation when requested, then record
owner acceptance with that exact commit/hash in `.execution/baseline-acceptance.md`.
All implementation tasks remain unstarted until that decision.

## Ubuntu toolchain

```bash
sudo apt update
sudo apt install -y build-essential curl git ca-certificates
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs -o /tmp/rustup-init.sh
sh /tmp/rustup-init.sh
source "$HOME/.cargo/env"
rustup component add rustfmt clippy
rustc --version
cargo --version
cc --version
```

If rustup already exists, inspect its installed toolchains instead of reinstalling.
T001 pins the selected stable version. LLVM is only needed at M05; do not choose
an arbitrary system LLVM major now. Benchmark runtimes/API access come at T034.

## OpenCode use

The previously installed Wolio skills, slash commands and global workflow have
been removed at owner request. No project-local OpenCode skills or commands are
installed. Use natural language: "Kerjakan T001 sesuai blueprint dan catat bukti."
Root AGENTS.md carries the repository instructions. Restart OpenCode after removal
so existing sessions no longer retain the previously loaded configuration.
