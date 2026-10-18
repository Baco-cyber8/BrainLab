[README-BrainLab-v0.6.5.md](https://github.com/user-attachments/files/33633154/README-BrainLab-v0.6.5.md)
# BrainLab

**BrainLab v0.6.5** is a command-line toolkit for organizing and running systematic alpha research workflows with a user's own WorldQuant BRAIN account. It supports reusable alpha templates, controlled expression variants, simulation queues, results review, correlation checks, and CSV exports.

- **Repository:** https://github.com/Baco-cyber8/BrainLab
- **Release:** https://github.com/Baco-cyber8/BrainLab/releases/tag/v0.6.5
- **Command reference:** [`docs/COMMANDS.md`](docs/COMMANDS.md)
- **Python:** 3.10 or newer
- **Dependency:** `requests`

> **Important:** BrainLab is an independent community project. It is not an official WorldQuant product and is not endorsed by WorldQuant. It does not guarantee that generated alphas will pass platform checks, be accepted, or perform profitably.

## What BrainLab does

- Connects to the BRAIN API using the user's own account.
- Retrieves account-supported options and field/operator metadata.
- Stores reusable alpha templates and their settings.
- Generates controlled variants of alpha expressions.
- Queues and runs simulations with configurable budgets and concurrency.
- Displays simulation results and platform checks.
- Helps review self-correlation and production-correlation checks.
- Exports research results to CSV.
- Checks whether an alpha appears ready for the user to submit manually.

**BrainLab does not submit alphas automatically.** The `brainlab submit ID` command is a readiness check only. The user must review and submit the alpha through the BRAIN website.

## Requirements

- Python 3.10 or newer.
- An internet connection for login, metadata retrieval, simulations, and platform checks.
- A WorldQuant BRAIN account with access to the relevant features.
- Git if you choose to install by cloning the repository.
- A terminal or command-line shell.

Only install and use BrainLab with an account you are authorized to access. Platform access and available options may differ by account.

## Download BrainLab v0.6.5

You can get the current release from the **[BrainLab v0.6.5 Releases page](https://github.com/Baco-cyber8/BrainLab/releases/tag/v0.6.5)**.

1. Open the release page.
2. Under **Assets**, download the BrainLab ZIP archive.
3. Extract the ZIP file.
4. Open a terminal in the extracted `BrainLab` project folder.
5. Follow the instructions for your operating system below.

You can also browse the source code on the [BrainLab GitHub repository](https://github.com/Baco-cyber8/BrainLab).

## Installation

### Android using Termux

Install Termux from a trusted source and open it. Run:

```sh
pkg update
pkg install python git
git clone https://github.com/Baco-cyber8/BrainLab.git
cd BrainLab
python -m pip install -r requirements.txt
sh install.sh
brainlab --version
```

If you downloaded and extracted the ZIP instead of cloning, skip the `git clone` command and use `cd` to enter the extracted `BrainLab` folder.

The installer places the `brainlab` launcher in `$PREFIX/bin` when Termux sets `PREFIX`. If the command is not found after installation, check that the install directory is on your `PATH`. You can also run commands from the project directory using:

```sh
python -m brainlab --help
```

For long-running background jobs, Android battery management may stop Termux processes. Termux:API and `termux-wake-lock` may help, but device settings vary.

### Linux and macOS

Install Python 3.10+ and a terminal shell. Git is needed only if cloning the repository. Then run:

```sh
git clone https://github.com/Baco-cyber8/BrainLab.git
cd BrainLab
python3 -m pip install -r requirements.txt
sh install.sh
brainlab --version
```

If you downloaded the ZIP instead, extract it and open a terminal in the extracted `BrainLab` directory before running the dependency-install and installer commands.

The installer places the launcher in `$HOME/.local/bin` by default. Make sure that directory is on your `PATH`. If you do not want to install the launcher, run commands from the project directory with:

```sh
python3 -m brainlab --help
```

### Windows using PowerShell

Install Python 3.10+ and download or extract the BrainLab source ZIP. Open PowerShell in the extracted `BrainLab` project directory and run:

```powershell
py -m pip install -r requirements.txt
py -m brainlab --version
py -m brainlab --help
```

Run other commands by replacing `brainlab` with `py -m brainlab`, for example:

```powershell
py -m brainlab login
py -m brainlab status
py -m brainlab options
```

The included `install.sh` is a shell script, so the instructions for Termux/Linux/macOS do not apply directly to standard Windows PowerShell. Windows behavior should be verified on the user's own system.

## First run

Run these steps in order. Commands that access the platform require an internet connection and valid account access.

### 1. Check the CLI

On Termux/Linux/macOS, if the launcher is installed:

```sh
brainlab --version
brainlab --help
```

On Windows, or if you are running from the project directory without the launcher:

```sh
python -m brainlab --version
python -m brainlab --help
```

On Windows, use `py -m brainlab` instead of `python -m brainlab` if that is how Python is configured on your system.

### 2. Log in

```sh
brainlab login
```

Use `python -m brainlab login` (or `py -m brainlab login` on Windows) if you are not using the installed launcher. Follow the prompts.

To check whether the saved session is still valid:

```sh
brainlab login --check
```

BrainLab stores local state under `~/.brainlab/` by default, or under the directory configured by `BRAINLAB_HOME`. Keep this directory private. Do not upload or commit credentials, session data, logs, databases, or private research.

### 3. Check account-supported options

```sh
brainlab options
```

This asks the platform which regions, universes, delays, and related options your account can use. Do not assume every account has the same options.

### 4. Retrieve field and operator metadata

```sh
brainlab fields --refresh --by-dataset
brainlab fields --stats
brainlab operators --refresh
```

These commands access the API and may take time. Field caches are scoped to the instrument type, region, universe, and delay.

### 5. Run one simulation first

Create a template and simulate it once:

```sh
brainlab template add momentum "rank(ts_mean(close, 20))" --neutralization SUBINDUSTRY --decay 4
brainlab sim momentum
brainlab results
```

A simulation makes a real request to BRAIN and may use your account's simulation allowance. Start with one simulation before generating a larger batch.

### 6. Preview variants before running them

Preview generation without adding jobs to the queue:

```sh
brainlab generate momentum --limit 10 --dry-run
```

If the preview looks correct, queue and run a small batch:

```sh
brainlab generate momentum --limit 10
brainlab run --budget 10 --concurrency 1
brainlab results
```

Use small budgets and low concurrency until you understand the workflow and your account's limits.

## Typical research workflow

A typical sequence is:

```sh
brainlab status
brainlab template list
brainlab generate momentum --limit 20 --dry-run
brainlab generate momentum --limit 20
brainlab run --budget 20 --concurrency 1
brainlab results --candidates
brainlab check --candidates --limit 10
brainlab show 1
brainlab export --candidates --out candidates.csv
```

Replace example names and IDs, such as `momentum` and `1`, with the template name and job ID from your own installation. See the [complete command reference](docs/COMMANDS.md) for all commands and flags.

## Reviewing and submitting an alpha

Review the relevant job and its checks before making a decision:

```sh
brainlab check 42
brainlab show 42
brainlab review 42 --accept --note "Reviewed checks and correlation results"
brainlab submit 42
```

Replace `42` with the correct job ID. The readiness command does not submit anything. If the alpha is ready, open the returned link and complete the submission on the BRAIN website yourself. After submitting there, you can record the status locally:

```sh
brainlab sync-submitted 42
```

## Useful commands

```sh
brainlab status
brainlab settings
brainlab options
brainlab fields --search close --limit 20
brainlab template list
brainlab results --sort sharpe
brainlab results --candidates
brainlab show 42
brainlab check --candidates --limit 10
brainlab export --candidates --out candidates.csv
brainlab --help
brainlab COMMAND --help
```

See [`docs/COMMANDS.md`](docs/COMMANDS.md) for the full command reference.

## Configuration and local data

By default, BrainLab stores local data in `~/.brainlab/`:

- `brainlab.sqlite3` — local job and research database.
- `config.json` — setting overrides.
- `credentials.json` and `session.json` — sensitive account/session data.
- `brainlab.log` — local log.

`BRAINLAB_HOME` can be used to choose another data directory. The tool also recognizes `BRAIN_CREDENTIAL_EMAIL`, `BRAIN_CREDENTIAL_PASSWORD`, and `BRAIN_API_URL`.

Avoid putting passwords directly in shell history or scripts. Never commit real credentials, session files, databases, logs, or private research exports.

## Troubleshooting

- **`brainlab: command not found`** — run `sh install.sh`, check that the install directory is on `PATH`, or use `python -m brainlab` from the project directory.
- **`No module named requests`** — run `python -m pip install -r requirements.txt`.
- **Login or session errors** — run `brainlab login --check`, then log in again if needed. Follow any verification steps required by the platform.
- **No fields or an invalid region** — run `brainlab options` and select options supported by your account.
- **`Queued 0 new`** — matching variants may already exist in your job history. Review the command reference for ways to change the search scope or lookback windows.
- **Rate limits or repeated authentication failures** — reduce concurrency, check the platform status and account state, and retry responsibly.
- **Windows command differences** — use `py -m brainlab COMMAND` in PowerShell.

## Security and responsible use

- Use only an account you are authorized to access.
- Respect the platform's terms, API rules, rate limits, and verification controls.
- Do not attempt to bypass multi-factor authentication, CAPTCHA, rate limits, or access controls.
- Keep credentials and session files private and out of source control.
- Start with low simulation budgets and concurrency.
- Review platform checks and correlation results; generated candidates are not guaranteed to pass or perform well.
- Historical simulation results do not guarantee future performance.

## Tests

The project includes an offline test suite. From the repository root, install the dependencies and run:

```sh
python -m unittest discover -s tests -t .
```

These tests check local behavior and fixtures; they do not prove that the live API is available or that every platform operation will succeed.

## Licensing and third-party materials

No project license is currently included. Until the project owner adds a license, do not assume that others have permission to redistribute or publish modified copies of BrainLab.

The current v0.6.5 archive includes ACE API reference materials under `vendor/`. Their applicable terms and redistribution permissions should be reviewed before distributing modified packages or republishing those files.

WorldQuant BRAIN and related names are used to identify the third-party platform. BrainLab is independent and is not endorsed by or affiliated with WorldQuant.
