# BrainLab
A command-line toolkit for WorldQuant BRAIN alpha research and experimentation.

BrainLab is a Python-based command-line interface designed to help users organize and manage alpha research workflows with WorldQuant BRAIN.

The project aims to make common research tasks more accessible through terminal commands, with support for configuration, data-field discovery, alpha generation, simulation, and results review.

Features

- Configuration: Manage BrainLab settings.
- Data discovery: Explore available datasets, data fields, and operators.
- Alpha generation: Generate alpha expressions using supported workflows.
- Simulation: Run alpha simulations through the configured backend.
- Results review: Inspect simulation results and performance information.
- Research workflow: Organize alpha experiments from the command line.
- Cross-platform goal: Designed for terminal-based use on Windows, Linux, and Android through Termux.

Feature availability depends on the installed version and your WorldQuant BRAIN account access.

Requirements

- Python 3
- An internet connection for operations that access online services
- A valid WorldQuant BRAIN account for account-dependent functionality
- The dependencies included with the downloaded release

Download

To get BrainLab:

1. Open this repository on GitHub.
2. Click Releases.
3. Open the latest release.
4. Download the ZIP file attached to that release.
5. Extract the ZIP file to a folder on your computer.

Older versions, when available, can be found under the repository's other releases.

Use the ZIP attached to the release you want. Do not assume that every release has the same features or requirements.

Installation and Usage

Windows PowerShell

Open PowerShell and navigate to the extracted BrainLab directory.

For example, if you extracted it into your Downloads folder:

cd "$HOME\Downloads\BrainLab"

Install the required dependencies if a "requirements.txt" file is included:

py -m pip install -r requirements.txt

Display the available commands:

py -m brainlab --help

Check the installed version:

py -m brainlab --version

Linux

Open a terminal and navigate to the extracted directory:

cd ~/Downloads/BrainLab

Install the dependencies if "requirements.txt" is included:

python3 -m pip install -r requirements.txt

Display the available commands:

python3 -m brainlab --help

Android Termux

Install Python in Termux if it is not already installed:

pkg update
pkg install python

Navigate to the directory where you extracted BrainLab. The following is an example; your actual path may differ:

cd ~/BrainLab

Install the dependencies if "requirements.txt" is included:

python -m pip install -r requirements.txt

Display the available commands:

python -m brainlab --help

Important: The directory name and commands may differ between releases. Follow any release-specific instructions if they are provided.

Getting Started

After installation, begin by checking the available commands:

python -m brainlab --help

Depending on your installed version, you can explore commands for settings, data fields, operators, alpha generation, simulations, and results.

For example, versions that support these commands may allow you to inspect settings and available options using:

python -m brainlab settings --help
python -m brainlab fields --help
python -m brainlab generate --help
python -m brainlab run --help

Check the help output for your installed version before running a command. Not every command or option is necessarily available in every release.

Important Notes

- BrainLab is a research workflow tool, not a guarantee of profitable alphas.
- Simulation results depend on the expression, dataset, settings, and platform behavior.
- You are responsible for reviewing alpha performance and compliance with the applicable platform requirements.
- Account credentials, passwords, session cookies, API tokens, and other secrets should never be committed to a public repository.
- Do not assume that generating or simulating an alpha automatically submits it to WorldQuant BRAIN.
- Use only software and third-party components that you are authorized to use and redistribute.

Versions and Releases

BrainLab versions are distributed through GitHub Releases.

Each release should identify its version and include the corresponding ZIP archive. Older releases may be retained so users can download a version that meets their needs.

Check the release notes before upgrading or switching versions.

Contributing

Contributions, bug reports, and suggestions are welcome.

When reporting a problem, include:

- The BrainLab version
- Your operating system
- The command you ran
- The complete error message, with credentials and other sensitive information removed

Disclaimer

BrainLab is an independent research tool. It is not an official WorldQuant product unless explicitly stated otherwise, and no affiliation or endorsement should be inferred.

Use of WorldQuant BRAIN remains subject to the platform's applicable terms, rules, and policies.
