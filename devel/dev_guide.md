<!-- vim-markdown-toc GFM -->

* [Develop Environment Setup](#develop-environment-setup)
    * [Helper script for git actions](#helper-script-for-git-actions)
    * [Python code formatter](#python-code-formatter)
    * [Python code checker](#python-code-checker)
    * [Command tools for github pull requests](#command-tools-for-github-pull-requests)
    * [NFS service](#nfs-service)
* [Testing Environment Setup](#testing-environment-setup)
    * [Script for creating test veth](#script-for-creating-test-veth)
    * [Script for clean up](#script-for-clean-up)
    * [Script for testing without container](#script-for-testing-without-container)
    * [Script for testing inside container](#script-for-testing-inside-container)
    * [Script for testing inside slow container](#script-for-testing-inside-slow-container)
* [Checklist for contributors](#checklist-for-contributors)
    * [Git commit Sign-off](#git-commit-sign-off)
    * [Commit message format](#commit-message-format)
    * [CI passing](#ci-passing)
    * [Create Github pull request](#create-github-pull-request)

<!-- vim-markdown-toc -->

## Develop Environment Setup

After create fork of https://github.com/nmstate/nmstate and clone to local.
Below sections are tips to boost the develop environment.

### Helper script for git actions

 * [Sync with upstream repo - `git_sync_upstream`][git_sync_upstream]
 * [Rebase current branch with base branch - `rebase`][rebase]

### Python code formatter

Please install the python code formatter - [black][black_url].
The CI will check whether code looks good according to black, hence before
creating pull request, use `black .` in your code base could fix them up before
CI complains.

You may add below lines into your `.vimrc` which allows you to format the
python code in vim by pressing `\` then `f`.
The [vim-plug][vim_plug_url] is required.

```
let g:black_linelength = 79
let g:black_skip_string_normalization = 0
autocmd FileType python nnoremap <silent> <leader>f :Black<cr>

call plug#begin('~/.vim/plugged')
if $USER != 'root'
"Plug 'psf/black', { 'tag': 'stable' }
Plug 'psf/black'
endif
```

### Python code checker

After install pylint and flake8, you should use [this script][cs] to check your
code before submitting pull request.

### Command tools for github pull requests

By installing [github command line tool][hub_cli], you could simplify the work
on creating and reviewing PR:
* Create PR: `hub pull-request -l Wait_Review`
* Create backport PR: `hub pull-request -l Wait_Review -b nmstate-0.3`
* Pull PR for review: `hub pr checkout <PR-NUMBER>`

### NFS service

Testing nmstate will lose internet access, so we need to share the code
to test VMs. NFS service running at develop machine would be enough:

```
/home/fge/Source    192.168.122.0/24(rw,sync,no_subtree_check,no_root_squash)
```

## Testing Environment Setup

Please create these VMs for testing purpose of nmstate:
 * RHEL/CentOS 8 as main test environment
 * Fedora
 * Ubuntu 18.04

All of them should be NFS access to develop machine code and this line in
`~/.bashrc`:

```bash
export PYTHONPATH=/home/fge/Source/nmstate
```

And [this file][nmstatectl] to `/usr/bin/nmstatectl` and create a soft link to
`/usr/bin/ncl`.

### Script for creating test veth

The [`ci_env`][ci_env] will setup eth1 and eth2 for testing.
The `ci_env 1` will remove them.

### Script for clean up

Please download [`ct`][ct] and edit `$NIC` to the NIC of VM and backup the
network state by `sudo ncl show $NIC > ~/os.yml`.


### Script for testing without container

The [`pt`][pt] script can be used by:

```bash
cd Source/nmstate/test/integration
pt -x
```

To run the pt with runtime logging: [`pt_debug`][pt_debug]
To run the pytest multiple times until fail: [`pt_repeat`][pt_repeat]

```bash
pt_repeat 100 -x
```

### Script for testing inside container

Download [`ci_docker`][ci_docker] and place it into your $PATH.

Download [`docker_cmds`][docker_cmds] and modify it to the correct git repo
path.

To start a container:

```
ci_docker
```
After that, copy the content of `docker_cmds` and paste it in the container
shell.

### Script for testing inside slow container

Just use [`ci_docker_slow`][ci_docker_slow], or add `cpu=0.1` after `podman`
or `docker` command.

To simulate the travis CI, the `ci_docker_slow` should be run on Ubuntu 18.04.

## Checklist for contributors
### Git commit Sign-off

This line should included at the bottom of git commit message.

```
Signed-off-by: John Doe <jdoe@example.com>
```

The `git commit -s` will do the magic.

### Commit message format

The commit message format should be:

```
section: brief description

Full descritption including root cause analyze, code workflow, test coverage.
```

Example:

```
ovsdb: Preserve the NM external_ids

For newly created OVS internal interface with customer external_ids,
the ovsdb plugin will remove the NM external_ids `NM.connection.uuid`.

The fix is read the current `NM.connection.uuid` before applying
configure and merge it with original desired state.

Integration test cases added.
```

### CI passing

The CI should be pass before any patch been accepted.
Instead of waiting CI, you should try to run the test locally mentioned above.

### Create Github pull request

If possible, please add `Wait_Review` flag to a pull request.
Other flags:
 * `Waiting_Rebase`: PR approved. Need rebase.
 * `Wait_on_submitter`: Changes is required.
 * `Work_Ongoing`: Work in progress.
 * `backport candidate`: Need backport to stable branch.
 * `Wait_Other_Response`: Waiting other project's response.
 * `Wait_Other_PR`: Depend on other PR.

[hub_cli]: https://github.com/github/hub
[black_url]: https://github.com/psf/black
[vim_plug_url]: https://github.com/junegunn/vim-plug
[git_sync_upstream]: ../utils/git_sync_upstream
[rebase]: ../utils/rebase
[ci_docker]: ../utils/ci_docker
[ci_docker_slow]: ../utils/ci_docker_slow
[ci_env]: ../utils/ci_env
[pt]: ../utils/pt
[ct]: ../utils/ct
[docker_cmds]: ../utils/docker_cmds
[nmstatectl]: ../utils/nmstatectl
[pt_debug]: ../utils/pt_debug
[pt_repeat]: ../utils/pt_repeat
