PyPilot and nodefit find the Python or Node.js release that every dependency of a project can run on. When no release fits, they name the packages that clash.

## PyPilot

Package metadata often leaves out which Python versions work. mediapipe 0.10.14 declares no `requires_python` at all and ships wheels only for Python 3.9 to 3.12, so on 3.13 pip finds nothing to install, and its error never mentions the Python version. PyPilot checks each dependency's declared range, when there is one, against the Python versions it has wheels for on your platform. It then sets up the venv with uv on a version they all support.

When an environment breaks, `pypilot doctor --fix` applies the fix that problem needs. A missing package gets a `uv sync` into the existing venv, and the venv is rebuilt only when it is broken itself. PyPilot also picks the torch and tensorflow builds that match your NVIDIA driver. It works from PyPI metadata and its own data tables, so there is no AI model involved and no API key to set up.

| Repository | What it is | Install |
|---|---|---|
| [pypilot](https://github.com/pypilot-dev/pypilot) | The Rust engine (the `pypilot` CLI and language server) and the Zed extension | In Zed, run `zed: extensions` and search for PyPilot |
| [pypilot-vscode](https://github.com/pypilot-dev/pypilot-vscode) | The VS Code extension | [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=AbdullahMasood-005.pypilot-vscode) or [Open VSX](https://open-vsx.org/extension/AbdullahMasood-005/pypilot-vscode) |
| [pypilot-pip](https://github.com/pypilot-dev/pypilot-pip) | The `pypilot` command and a Python API, on PyPI as `pypilot-cli` | `pip install pypilot-cli` |

Documentation: <https://pypilot-dev.github.io/pypilot/>

## nodefit

[nodefit](https://github.com/pypilot-dev/nodefit) does the same for Node.js. It reads each package's `engines.node` range and, for native modules such as sharp or better-sqlite3, the Node releases that have a prebuilt binary for your platform. You find out before `npm install` falls back to node-gyp and fails with compiler errors.

Run it with `npx @pypilot-dev/nodefit doctor`. The VS Code extension is a `.vsix` on the [releases page](https://github.com/pypilot-dev/nodefit/releases).
