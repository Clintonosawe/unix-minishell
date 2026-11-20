# miniShellOS
This is a small Unix shell project written in C, with a built-in AI helper you can ask questions without leaving the terminal.

We built it to learn how a shell works under the hood: parsing input, creating processes with `fork()` and `execvp()`, and wiring up pipes and redirection. On top of that, there's an `ai` command that sends your question to a language model running locally through GPT4All.

Built by Clinton Osawe and Ayuub Hagi.

## Features

- **Command execution:** parses your input and runs each command in a child process using `fork()`, `execvp()`, and `wait()`
- **Pipes:** chain commands together, like `ls -l | less`
- **Redirection:** send output to a file with `>` or `>>`, or read input from one with `<`
- **History:** type `!!` to rerun your last command
- **AI helper:** ask something like `ai "What does the execvp() system call do?"` and get an answer from a local model

## Requirements

- Linux or macOS
- GCC (7.5.0 or newer recommended)
- Python 3.10+
- Python packages: `gpt4all`, `transformers`, `torch`
- The `q4_0-orca-mini-3b.gguf` model file (not included in the repo, see setup below)

## Setup

1. Clone the repo:

   ```bash
   git clone https://github.com/Clintonosawe/unix-minishell.git
   cd unix-minishell
   ```

2. Create a virtual environment and install the Python packages:

   ```bash
   python3 -m venv venv
   source venv/bin/activate
   pip install --upgrade pip
   pip install gpt4all transformers torch
   ```

3. Download the model. It's too large to keep in the repo, so follow the steps in `MODEL_SETUP.txt` and save it here:

   ```
   models/q4_0-orca-mini-3b.gguf
   ```

4. Compile the shell:

   ```bash
   gcc main.c ai_handler.c shell_core.c utils.c -o smartshell
   ```

## Usage

Start the shell:

```bash
./smartshell
```

From there, it works like a regular shell:

```
cd projects
ls -l | less
echo "hello" > out.txt
!!
```

To ask the AI helper something, type `ai` followed by your question in quotes:

```
ai "What does the execvp() system call do?"
```

The shell passes your question to `ai_helper.py`, which runs it through the local model and prints the answer. Make sure your virtual environment is active before you start the shell.

## Troubleshooting

- **The `ai` command isn't working:** check that your virtual environment is activated.
- **Model not found:** make sure the file is at `./models/q4_0-orca-mini-3b.gguf`.
- **Model is slow to load or fails:** language models use a lot of memory, so check how much RAM you have free.
- **Anything else model-related:** see `MODEL_SETUP.txt` for how to get a compatible model.

## Notes

- `venv/` and `models/` are in `.gitignore`, so they won't be committed..