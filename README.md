[![CI-Build-Pipeline](https://github.com/shirokhorshid/psiphon-tunnel-core/actions/workflows/build.yml/badge.svg)](https://github.com/shirokhorshid/psiphon-tunnel-core/actions/workflows/build.yml)

Psiphon Tunnel Core (Console Client Fork)
================================================================================

Overview
--------------------------------------------------------------------------------

This repository is a specialized fork of the Psiphon Tunnel Core project, focusing specifically on cross-compiling, configuring, and optimizing the standalone command-line **Console Client** (`ConsoleClient`). 

Unlike the upstream repository, this project includes:
- **Automated Multi-Architecture Builds:** GitHub Actions workflows compiled automatically for Linux, Windows, and macOS architectures on every upstream change.
- **Advanced Domain Fronting Configuration:** Pre-optimized parameters specifically tuned for restrictive networking environments utilizing Domain Fronting, `FRONTED-MEEK` variants, and `IndistinguishableTLS`.
- **System Automation:** Native systemd service wrappers for headless deployment on home servers and remote virtual private servers (VPS).
