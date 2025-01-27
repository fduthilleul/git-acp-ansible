# git-acp-ansible

`git_acp` is an Ansible module for `git add`, `git commit`, `git push` and `git config` operations on local or remote (https/ssh) git repo.

### PyPi Install:

PyPi package is not longer supported (last version available is `1.1.2`). Using collection is strongly advised.
For older Ansible versions that do not support collection, you can copy `ansible_collections/lvrfrc87/git_acp/plugins/modules/git_acp.py` into `library` directory in the root of your Ansible project:

```
myproject/
├── ansible.cfg
├── inv/
├── library/
│   ├── git_acp.py
├── playbooks/
├── roles/
```


### Ansible Galaxy Install (for Ansible version > 2.9)

All info related to Ansible Galax install are available [here](ansible_collections/lvrfrc87/git_acp/README.md)

### Module Documentation:

```
module: git_acp
options:
    path:
        description:
            - Folder path where C(.git/) is located.
        required: true
        type: path
    comment:
        description:
            - Git commit comment. Same as C(git commit -m).
        type: str
        required: true
    add:
        description:
            - List of files under C(path) to be staged. Same as C(git add .).
              File globs not accepted, such as C(./*) or C(*).
        type: list
        elements: str
        default: ["."]
    user:
        description:
            - Git username for https operations.
        type: str
    token:
        description:
            - Git API token for https operations.
        type: str
    branch:
        description:
            - Git branch where perform git push.
        required: True
        type: str
    push_option:
        description:
            - Git push options. Same as C(git --push-option=option).
        type: str
    mode:
        description:
            - Git operations are performend eithr over ssh, https or local.
              Same as C(git@git...) or C(https://user:token@git...).
        choices: ['ssh', 'https', 'local']
        default: ssh
        type: str
    url:
        description:
            - Git repo URL.
        required: True
        type: str
    remote:
        description:
            - Local system alias for git remote PUSH and PULL repository operations.
        type: str
        default: origin
    user_name:
        description:
            - Explicit git local user name. Nice to have for remote operations.
        type: str
    user_email:
        description:
            - Explicit git local email address. Nice to have for remote operations.
        type: str
```

### Examples:

```
- name: HTTPS | add file1.
  git_acp:
    user: Federico87
    token: mytoken
    path: /Users/git/git_acp
    branch: master
    comment: Add file1.
    remote: origin
    add: [ "." ]
    mode: https
    url: "https://gitlab.com/networkAutomation/git_test_module.git"

- name: SSH | add file1.
  git_acp:
    path: /Users/git/git_acp
    branch: master
    comment: Add file1.
    add: [ file1  ]
    remote: dev_test
    mode: ssh
    url: "git@gitlab.com:networkAutomation/git_test_module.git"
    user_name: lvrfrc87
    user_email: lvrfrc87@gmail.com

- name: LOCAL | push on local repo.
  git_acp:
    path: "~/test_directory/repo"
    branch: master
    comment: Add file1.
    add: [ file1 ]
    mode: local
    url: /Users/federicoolivieri/test_directory/repo.git
```
