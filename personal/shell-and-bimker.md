# Shell Configuration and Bimker

Personal host guidelines. These extend the shared
[Host System Interaction](../shared/generic-agent-guidelines.md#host-system-interaction)
rules.

## Shell Configuration

This user uses the [bimhaw](https://github.com/salotz/bimhaw) tool for managing shell configuration. In this framework the standard `.profile`, `.bashrc`, etc. are generated and managed by a central configuration that brings in modules for different tool configuration.

If a new shell integration is required these guidelines should be obeyed.

The directory to use for configuration is `~/.bimker` (see [Bimker](#bimker)).

## Bimker

Bimker is just a nonsense name (loosely based on the word "bunker") used to refer to the git tracked repo which contains all of my personal host configuration.

This directory is always located at `~/.bimker`. Agents should read the context in that directory/repo for understanding how to make changes there if necessary.

Changes to Bimker should be treated with special care and first proposed and made explicit exactly what changes are being made and why. Do not attempt to refactor or change the structure unless specifically asked and currently working on Bimker as the project under work.

Of particular interest is the shell configuration which is at `~/.bimker/bimhaw`. Refer to the [bimhaw](https://github.com/salotz/bimhaw) documentation for instructions on modifying this for all shell configuration tasks.
