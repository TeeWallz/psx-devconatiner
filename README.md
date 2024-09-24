# VSCode Devcontainer for PSX Development

This is a VSCode devcontainer for PSX development using PSN00bSDK. It is based on the Ubuntu devcontainer provided by Microsoft. The devcontainer is setup to build and debug a sample project provided by PSN00bSDK.

Trying to get this consistently running on my host machine was a pain and I kept forgetting how it was set up. I'm a fan of NixOS and I wanted to have a consistent development environment. This is my attempt to make it easier for myself and others to get started with PSX development.

## Setup

- Install PCSX-Redux on host
- Make sure PCSX-Redux setting GDB Server Port is set to 3333
- Set your host's IP in .vscode/launch.json `target`
- If a different demo is desired, change executable and load .elf files in .vscode/launch.json
- Open dev container
- Untill TODO is finished, run below commands in dev container to install VScodes recommended extensions
  - `./.devcontainer/install-vscode-extensions.sh`
- Debug using F5
- The sample project should build and open in PCSX Redux

## TODO

- [ ] How to get VSCode to respect the default extension settings
- [ ] Include PsyQ SDK sample project setup

## Directories of interest

- PSN00bSDK location=/opt/psn00bsdk
- Sample project location /opt/sample_projects/psn00bsdk/
- /workspace - VSCode workspace. Not forced to be sample project location

## Sources

- Devcontainer - https://github.com/microsoft/vscode-dev-containers/tree/main/containers/ubuntu/.devcontainer
- CMake - https://askubuntu.com/questions/355565/how-do-i-install-the-latest-version-of-cmake-from-the-command-line
- PSN00bSDK setup - https://github.com/Lameguy64/PSn00bSDK/blob/master/doc/installation.md
- PSN00bSDK cmake setup - https://github.com/Lameguy64/PSn00bSDK/blob/master/doc/cmake_reference.md#setup

## How to build docker image manually to troubleshoot

```bash
docker build -t psx-devcontainer ./.devcontainer
docker run -it psx-devcontainer
```