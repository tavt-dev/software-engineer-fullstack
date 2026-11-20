# Repository guidance

## Purpose and structure

This repository records a Vietnamese learning journey toward software engineering and fullstack development. It currently contains study notes and practical exercises for Git, networking, Linux, and Docker, rather than an executable application.

- `README.md`: learning roadmap, progress, and links to topic indexes.
- `git/README.md`: Git and GitHub notes and workflow examples.
- `network/`: networking notes and `exercise/` submissions.
- `linux/`: numbered lessons, `exercise/` submissions, and `tai-lieu/` reference PDFs.
- `docker/`: numbered lessons, `exercise/` submissions, and `tai-lieu/` reference PDFs.
- `img/`: shared diagrams; topic-specific exercise screenshots live in `exercise/img/`.

## Editing content

- Write learning material in Vietnamese with proper diacritics. Keep technical terms, commands, and identifiers accurate and consistent with surrounding material.
- Explain concepts for beginners using concrete examples. Follow the nearby lesson or exercise format rather than imposing a new template.
- Use Markdown headings, tables for command references or comparisons, and fenced code blocks with a suitable language such as `bash`.
- Follow existing filenames: numbered lessons use Vietnamese names without diacritics, such as `05-tep-va-thu-muc.md`; exercises use the naming pattern in their topic directory.
- Link to local files and images using relative paths. Include meaningful image alt text. Preserve existing HTML image sizing where appropriate.
- Add new lessons and exercises to their topic's `README.md`. Update the root index when adding a topic or when requested progress changes affect the roadmap.
- Preserve issue references, screenshots, and recorded exercise outcomes. Do not invent execution results or mark work complete without evidence.
- Keep changes focused; avoid replacing existing PDFs or images unless needed for the task.

## Command examples and validation

There is no configured build, automated test suite, or lint command. Do not add application tooling solely to validate documentation.

- Review changed Markdown for readable headings, tables, fences, and working relative links and image paths.
- Check command syntax and explain prerequisites, working directories, ports, and resource names when needed. Distinguish Bash/Linux examples from Windows commands.
- Treat documented commands as instructional examples, not instructions to execute against the current machine. Commands that install packages, delete data, change services, or create containers require an appropriate task context before execution.
- Use lab-only credentials in examples; never include real secrets or personal server credentials.
- Run `git diff --check` and review `git diff` before finishing. Report what was validated and any examples that were not executed.

## Git conventions

Recent commits use descriptive prefixes such as `docs(docker):` and `practice(linux):`. Follow that style when a commit is requested. Existing Git notes describe `learning/` branches for notes and `practice/` branches for experiments; follow the user's chosen branch and preserve unrelated changes.
