# 42sh
42sh is a modular, custom Unix command-line interpreter developed in **C** as a team project at EPITECH Paris. Built as an evolution of our preliminary **minishell** project, **42sh** mimics key mechanisms, syntax, and built-in behaviors of standard shells - specifically taking inspiration from **'tcsh'**.

## Key Features and Evolution
**Foundation** (Inherited from Minishell - Individual Project)
(You can check my individual implementation in the `Minishell GitHub Repository`: [https://github.com/dianatrc/minishell])
- **PATH Resolution:** Automatically searches and executes binaries located within system directories defined in the `PATH` environment variable or via absolute/relative paths.
- **Environment Management:** Copies and safely restores the initial environment so that modifications do not affect the original env variable.
- **Core Built-in Commands:**
    - **`cd`:** Navigates directories with support for absolute/relative paths, `~` (home directory) and `-` (previous directory).
    - **`env`:** Displays the current environment variables list.
    - **`setenv`:** Dynamically adds or updates environment variables with strict alphanumeric name validation.
    - **`unsetenv:`** Removes one or multiple environment variables.
    - **`exit:`** Safely terminates the shell session and releases all allocated memory.
- **Exit Status Tracking `(echo $?)`:** Accurately propagates the return code of the last executed command or child termination signals.
- **Command Chaining (`;`):** Executes multiple sequential commands separated by semicolons on a single input line.
- **Pipes (`|`):** Connects the standard output of one process to the standard input of another using system pipes.
- **File Redirections:**
    - **Output Redirections (`>` and `>>`):** Redirects standard output to a file in overwrite or append mode.
    - **Input Redirection (`<`):** Reads process input directly from a specified file.
- **Syntax and Error Validation:** Comprehensive error checks detecting invalid pipe placements, ambiguous redirections and syntax anomalies matching standard tcsh behavior.

**Advanced 42sh Features**
- **Abstract Syntax Tree (AST):** Modular parser pipeline converting complex command lines into structured execution trees.
- **Heredoc (`<<`):** Multi-line inline input redirection with custom delimiter processing.
- **Inhibitors and Quotes:** Complete handling of a single quote (`'`), double quotes (`"`), and backslashes (`\`). 
- **History (`history`, `!`):** Command history tracking and re-execution mechanisms mimicking `tcsh`.
- **Globbing:** Dynamic file path expansion for wildcards and character sets.
- **Job Control:** Background process execution and foreground job management.
- **Multi-language Error Messaging (Bonus):** Custom localization system that outputs error messages in multiple languages depending on the shell configuration, with default being English.

## Project Overview
- 42sh-env/                     # Multi-languages error messages.
- bonus/
    - print_error/              # Display multi-languages error messages.
    - barcode
    - bonus.c                   # Custom prompt display.
- include/
    - alias.h                   # Alias structure and function prototypes.
    - editor.h                  # Header file for editor.
    - functions.h               # Header file for functions and for includes.
    - history.h                 # Header for the history command.
    - jobs.h
    - lang.h                    # Contains all error messages for several languages.
- src/ 
    - alias/
        - alias_builtins.c      # Handling alias logic.
        - alias_utils.c         # Alias helper functions.
        - string_arr_tools.c    # String array tool functions.
    - builtins/
        - builtins.c            # Builtin functions.
        - cd.c                  # Functions for moving through directories.
        - env_utils.c           # Functions for managing the enviroment variables.
    - history/
        - history_managememnt.c # Manage the history command and the associated flags.
        - history.c             # Manage the history command.
    - jobs/
        - background.c
        - get_jobs.c
        - jobs_control.c
        - jobs_struct.c         # Remove and add element to jobs_t struct.
        - manage_jobs.c         # Jobs control.
    - line_edition/
        - actions.c             # Functions to manipulate the editor_t struct.
        - binding.c             # Dynamic rebinding functions.
        - multiline.c           # Handle line reading and completion.
    - all_executions.c          # Manage all the execution of the given commands.
    - backticks.c               # Backticks handling.
    - exec.c                    # Execution and path handling functions.
    - free_tools.c
    - globbings.c               # Globbings handling.
    - inhibitors.c              # Manage all the inhibitors of 42sh.
    - main.c                    
    - parsing.c                 # Input parsing and command execution flow.
    - pipe.c                    # Pipe handling.
    - quote_inhibitor.c         # Manage 'the quote' of 42sh.
    - redirections.c            # Redirections handling.
    - special_variables.c       # Contains all functions to handle special variables.
    - split.c                   # Functions for && and || operators.
    - subshell.c                # Handling parentheses logic.
    - tools.c
- Makefile                      # Build system rules (all, clean, fclean, re).
- README.md

## Individual Contributions
While 42sh was developed as a group assignment, Minishell was built individually. Across these projects, my core contributions include:
1. **Custom Interactive Prompt**
- **Dynamic Formatting:** Displays a dynamic prompt, similar to `tcsh`, by showing the username (with capitalized first letter) and the current working directory with shortcut for home (User:~/path>). 

2. **Alias System `(alias / unalias)`**
- **Dynamic Command Substitution:** Implemented full alias definition, listing, and removal mechanics matching `tcsh` behavior.
- **Pre-parsing Expansion:** Designed the expansion layer to process aliases prior to tokenization.
- **Recursion and Infinite Loop Guard:** Built detection mechanisms to prevent infinite expansion loops during nested or self-referential aliases.

3. **Parenthese and Subshell Handling `(...)`**
- **Grammar and Operator Precedence:** Integrated parentheses evaluation into the parser to enforce operator precedence.
- **Subshell isolation:** Implemented subshell process spawning (`fork()`) to execute commands inside the parenthesized blocks in an isolated environment.

## Error Handling and Robustness
- **Syntax and Parsing Validation:** Detailed error checking for mismatched quotes, dangling operators (`&&`, `||`, `|`), invalid subshell syntax, and ambiguous redirections.
- **Signal Handling:** Intercepts `SIGINT` (Ctrl+C), `SIGTSTP` (Ctrl+Z), and child exit signals without crashing or corrupting the main terminal state.
- **Standardized Error Reporting**: Writes all runtime error messages directly to `stderr` and complies with standard exit code (exits with code 84 on critical failures or 0 on success).

## Compilation and Usage
**BUILD**
To compile the `42sh` binary using the provided Makefile, run: make. To clean intermediate or target files:
- `make clean`    # Removes object files.
- `make fclean`   # Removes object files and binaries.
- `make re`       # Clean rebuild.
**USAGE**
Run the shell interactively: `./42sh` or execute commands via standard input piping: `echo "(alias ll 'ls -l' && ll) | cat" | ./42sh`
