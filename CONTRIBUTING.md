# Contribution Guide

First of all, thanks for your interest and for taking the time to contribute! This document shall be your guide throughout the contribution process and will hopefully answer any questions you have.

## Table of Contents

- [Response Times](#response-times)
- [Reporting Bugs](#reporting-bugs)
- [Suggesting Enhancements](#suggesting-enhancements)
- [Contributing Changes](#contributing-changes)
    - [Prerequisites](#prerequisites)
    - [Development Environment](#development-environment)
    - [Conventions](#conventions)
        - [General](#general)
        - [Ansible Artifact](#ansible-artifact)
        - [Unix Shell Script](#unix-shell-script)
    - [Testing](#testing)
    - [Pull Request](#pull-request)
- [Code of Conduct](#code-of-conduct)
- [Questions](#questions)

## Response Times

This project has been made available to you without expecting anything in return. As a result, maintenance does not happen on a set schedule. Please keep in mind that the irregular maintenance schedule can lead to significant delays in response times.

## Reporting Bugs

Before reporting a bug, please check if the bug occurs in the latest version. If it does, and if it hasn't already been reported in the [bug tracker][1], feel free to [file a bug report][2].

## Suggesting Enhancements

> **Note:** Simplicity is a core principle of this project. Every enhancement suggestion is carefully evaluated and only accepted if the usefulness of the enhancement greatly outweighs any increase in complexity.

Before suggesting an enhancement, please ensure that the enhancement is not implemented in the latest version. In addition, please ensure that there is no straightforward alternative to achieve the desired outcome. If these conditions are met, and if the enhancement hasn't already been suggested in the [enhancement tracker][3], feel free to [file an enhancement suggestion][4].

## Contributing Changes

### Prerequisites

Before making changes that you plan to contribute, please follow these instructions:

- **Changes related to a [reported bug][1]:** Make sure that the bug has not yet been assigned to anybody (and that nobody has volunteered) and write a comment letting the community know that you have decided to fix the bug.
- **Changes related to an unreported bug:** [File a bug report][2].
- **Changes related to an [already suggested enhancement][3]:** Make sure that the enhancement has not yet been assigned to anybody and write a comment letting the maintainers know that you would like to implement the enhancement. Wait until the enhancement suggestion has been assigned to you.
- **Changes related to a not yet suggested enhancement:** [File an enhancement suggestion][4] and wait until it has been assigned to you.

Following these instructions keeps you (and others) from investing time in changes that would get rejected or are already being worked on.

### Development Environment

This project uses [Vagrant][5] to manage a portable development environment. Simply execute `vagrant up` inside the project's directory to start the setup. Once completed, you can access the development environment with `vagrant ssh`.

### Conventions

#### General

- Code *should* document itself.
- Code *must* be formatted by executing `vagrant ssh -c /mnt/project/sbin/format` inside the project's directory.

#### Ansible Artifact

- The [general conventions][6] *must* be followed.
- Lines longer than 80 characters *should* be avoided.
- The main task file (`tasks/main.yml`) *must* only contain includes of separate task files (`include_tasks`).
- Separate task files *must* be created for each area of responsibility.
- Names of separate task files *must* read as imperative verb phrases. For example: `create_swap_file.yml`, `manage_user.yml`.
- All tasks and blocks *must* be named.
- Task and block names *must* start with a capital letter and read as imperative verb phrases. For example: `Install editor`, `Check if user exists`.
- Tasks that need root privileges *must* use the `become` directive.
- Variable names *must* use snake case.
- Names of variables defined with the `register` directive *must* be suffixed with `_result`. If used together with the `loop` directive, they *must* be suffixed with `_results`.
- Loop variables *must* always be given a custom name.
- Handlers *must* be triggered using a `listen` topic.
- Handler `listen` topic names *must* use snake case.
- Handler `listen` topic names *must* be suffixed with `_handler` and read as noun phrases.

#### Unix Shell Script

- The [general conventions][6] *must* be followed.
- Lines longer than 80 characters *should* be avoided.
- Commands *must* be grouped and ordered as follows and groups *must* be separated from each other with an empty line:
    1. Environment checks (check if OS is supported, etc.)
    2. Shell option setting/unsetting
    3. File sourcing
    4. Function definitions
    5. Trap registrations
    6. Common variable assignments
    7. Main logic
- Functions *must* be separated from each other with an empty line.
- Function and variable names *must* use snake case.
- Names of functions that make modifications *must* read as imperative verb phrases. For example: `print_help`, `fork`.
- Names of functions that don't make modifications *must* read as [predicate phrases][7]. For example: `is_empty`, `exists`.
- Functions *must* be documented using Markdown syntax and following template:

    ```sh
    #---
    # Summary for function (if not obvious or description provided).
    #
    # Description for function (if extended documentation needed).
    #
    # @param $@ Description for all parameters (if needed).
    # @param $1 Description for parameter 1 (if needed).
    # @param... Description for remaining parameters (if needed).
    # @stdin Description for STDIN (if used).
    # @stdout Description for STDOUT (if used).
    # @stderr Description for STDERR (if used for non-error output).
    # @fd 3 Description for file descriptor 3 (if used).
    # @status Description for all status codes (if needed).
    # @status 1 Description for status code 1 (if needed).
    # @exit (if function calls `exit` outside of error cases)
    # @internal (if function is not intended for public use)
    func() { :; }
    ```

### Testing

This project uses [Molecule][8] to run tests. The tests are stored inside the `molecule` directory. Simply execute `vagrant ssh -c /mnt/project/sbin/test` inside the project's directory to run the test suite.

### Pull Request

Before creating a pull request, please follow these instructions:

- Ensure that the instructions in the [Prerequisites][9] and [Conventions][10] sections have been followed.
- Update the [test suite][11] and exercise the code you have written.
- Lint the codebase by executing `vagrant ssh -c /mnt/project/sbin/lint` inside the project's directory.
- Update the [README file][12].
- Update the [changelog][13].

After the pull request has been created, confirm that all [status checks][14] are passing. If you believe that a status check failure is a false positive, comment on the pull request and a maintainer will review the failure.

## Code of Conduct

Please note that this project is released with a [contributer code of conduct][15]. By participating in this project you agree to abide by its terms.

## Questions

Still have questions? No problem! Use the [question tracker][16] to [ask a question][17].

[1]: https://github.com/TobyGiacometti/AnsibleGroupwareServerRole/issues?q=is%3Aissue+label%3Abug
[2]: https://github.com/TobyGiacometti/AnsibleGroupwareServerRole/issues/new?template=bug.md
[3]: https://github.com/TobyGiacometti/AnsibleGroupwareServerRole/issues?q=is%3Aissue+label%3Aenhancement
[4]: https://github.com/TobyGiacometti/AnsibleGroupwareServerRole/issues/new?template=enhancement.md
[5]: https://www.vagrantup.com
[6]: #general
[7]: https://en.wikipedia.org/wiki/Predicate_(grammar)
[8]: https://github.com/ansible-community/molecule
[9]: #prerequisites
[10]: #conventions
[11]: #testing
[12]: README.md
[13]: CHANGELOG.md
[14]: https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-status-checks
[15]: CODE_OF_CONDUCT.md
[16]: https://github.com/TobyGiacometti/AnsibleGroupwareServerRole/issues?q=is%3Aissue+label%3Aquestion
[17]: https://github.com/TobyGiacometti/AnsibleGroupwareServerRole/issues/new?template=question.md
