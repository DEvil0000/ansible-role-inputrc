# Ansible Role: inputrc
Ansible role to configure inputrc

## Requirements
None

## Role Variables
The variables that can be passed to this role and a brief description about
them are as follows:

| Variable | Required | Default | Comments |
|-----------------------|----------|-----------|---------|
| `inputrc_write_global_config` | No | `true` | Set to true if the global config file should also be written. |
| `inputrc_global_file` | No | `/etc/inputrc` | The path to the global inputrc config. |
| `inputrc_file` | No | `.inputrc` | The filename of the users dot file. |
| `inputrc_users` | No | `['root']` | This defines for which users the inputrc should be written. |
| `inputrc_global_options` | No | `[...]` | The global inputrc options are defined here. |
| `inputrc_global_bindings` | No | `[...]` | The global inputrc bindings are defined here. |
| `inputrc_options` | No | `[]` | The users inputrc options are defined here. |
| `inputrc_bindings` | No | `[]` | The users inputrc bindings are defined here. |
| `inputrc_include` | No | `{{ inputrc_global_file }}` | The users inputrc will include this file. |

The defaults for `inputrc_global_options` are as follows:

```yaml
inputrc_global_options:
  # Be 8 bit clean.
  - name: input-meta
    value: on
  - name: output-meta
    value: on
```
Note: Those are the debian-family defaults.

The defaults for `inputrc_global_bindings` are as follows:

```yaml
inputrc_global_bindings:
  # allow the use of the Home/End keys
  - key: '\e[1~'
    function: beginning-of-line
    condition: mode=emacs
  - key: '\e[4~'
    function: end-of-line
    condition: mode=emacs
  # allow the use of the Delete/Insert keys
  - key: '\e[3~'
    function: delete-char
    condition: mode=emacs
  - key: '\e[2~'
    function: quoted-insert
    condition: mode=emacs
  # mappings for Ctrl-left-arrow and Ctrl-right-arrow for word moving
  - key: '\e[1;5C'
    function: forward-word
    condition: mode=emacs
  - key: '\e[1;5D'
    function: backward-word
    condition: mode=emacs
  - key: '\e[5C'
    function: forward-word
    condition: mode=emacs
  - key: '\e[5D'
    function: backward-word
    condition: mode=emacs
  - key: '\e\e[C'
    function: forward-word
    condition: mode=emacs
  - key: '\e\e[D'
    function: backward-word
    condition: mode=emacs
  # term rxvt things
  - key: '\e[7~'
    function: beginning-of-line
    condition: term=rxvt
  - key: '\e[8~'
    function: end-of-line
    condition: term=rxvt
  - key: '\eOc'
    function: forward-word
    condition: term=rxvt
  - key: '\eOd'
    function: backward-word
    condition: term=rxvt
```
Note: Those are the debian-family defaults.
Note: It is important not using the magic quotes for the key value.

## Examples

```yaml
- name: configure inputrc
  hosts: all
  tags:
    - inputrc
    - bashrc
    - bash
    - bind
  roles:
    - devil0000.inputrc
  vars:
    inputrc_users:
      - root
      - someone
    inputrc_options:
      - name: bell-style
        value: none
    inputrc_bindings:
      - key: '\e[A'
        function: history-search-backward
      - key: '\e[B'
        function: history-search-forward
```
