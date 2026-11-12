# gaudi-bash components

This repo is the home of the [gaudi-bash](https://github.com/g-udi/gaudi-bash) components. It holds no component itself: each kind lives in its own repository, included here as a git submodule.

| Folder | Repository | Holds |
| --- | --- | --- |
| `aliases` | [gaudi-bash-aliases](https://github.com/g-udi/gaudi-bash-aliases) | `lib/<name>.aliases.bash` |
| `completions` | [gaudi-bash-completions](https://github.com/g-udi/gaudi-bash-completions) | `lib/<name>.completions.bash` |
| `plugins` | [gaudi-bash-plugins](https://github.com/g-udi/gaudi-bash-plugins) | `lib/<name>.plugins.bash` |
| `themes` | [gaudi-bash-themes](https://github.com/g-udi/gaudi-bash-themes) | one folder per theme, each a submodule of its own |

`enabled/` holds the links gaudi-bash creates when a component is enabled, and `custom/` your own scripts; neither is tracked.

After cloning, fetch the submodules (gaudi-bash's installer does this for you):

```bash
git submodule update --init --recursive
```

The aliases, completions and plugins are the same files [gaudi-shell](https://github.com/g-udi/gaudi-shell) loads (it includes the three repositories as submodules directly), so they are written to work in bash 3.2, bash 5 and zsh. The lists below are generated from the README of each repository.

## Host contract

A component file never names a framework (no `GAUDI_BASH*` or `GAUDI_SHELL*` variable, no `gbash` or `gshell` command). It may only rely on the names both frameworks provide, each defined once in the framework's `lib/helpers/utils.bash` (the [gaudi-bash one](https://github.com/g-udi/gaudi-bash/blob/main/lib/helpers/utils.bash)):

| Name | Meaning |
| --- | --- |
| `cite`, `about-alias`, `about-plugin`, `about-completion`, `about`, `group`, `param`, `example`, `priority` | metadata read by `show`, `search`, `help` and `enable` |
| `_command_exists <name>` | true when `<name>` is anything the shell can run (function, alias, builtin or program); logs an error otherwise, so it is for things that must be there |
| `_binary_exists <name>` | true only when `<name>` is a program on `PATH`; a function or an alias does not count. The probe for an optional tool (use `command -v <name>` when a function should count too) |
| `_homebrew-check` | true when Homebrew is installed; sets `GAUDI_HOST_HOMEBREW_PREFIX` |
| `pathmunge <dir> [after]` | add a directory to `PATH` once |
| `_log_debug`, `_log_warning`, `_log_error` | logging |
| `_read_input <question>` | asks a yes/no question and leaves the answer in `REPLY` (`y` or `Y` means yes) |
| `_array-contains <item> <element>...` | true when `<item>` is one of the elements |
| `NC`, `BLACK`, `RED`, `GREEN`, `YELLOW`, `BLUE`, `MAGENTA`, `CYAN`, `WHITE` and their `BOLD_` forms | colour escape sequences, empty when colours are off; `NC` resets. Print them through a `printf` format string or `echo -e` |
| `GAUDI_HOST_NAME` | the framework's name (`gaudi-bash` or `gaudi-shell`), for messages and cache folders |
| `GAUDI_HOST_COMMAND` | the command its users type (`gbash` or `gshell`) |
| `GAUDI_HOST_HOME` | its install directory; `components/enabled` and `components/custom` live under it |
| `GAUDI_HOST_SED_I_PARAMETERS` | array with the arguments that make `sed` edit in place on this OS |
| `GAUDI_HOST_HOMEBREW_PREFIX` | the Homebrew prefix, empty until `_homebrew-check` or the brew plugin has found it |
| `_gaudi_host_setting <NAME>` | prints the user's setting `<NAME>` (for example `HTTP_PROXY`): `GAUDI_<NAME>`, otherwise the framework's own `GAUDI_BASH_<NAME>` or `GAUDI_SHELL_<NAME>` |

The README of each repository has the rules for writing a file (shell portability, nothing slow at load time, the header every file starts with).

## Themes

- [**gaudi**](https://github.com/g-udi/gaudi-theme): a modular prompt made of segments (working directory, git status, language versions, cloud profile, ...), with the slow ones rendered in the background. It ships with gaudi-bash; its settings are described in [its README](https://github.com/g-udi/gaudi-theme#readme).

## Plugins

- [**alias-completion**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/alias-completion.plugins.bash): Automatic completion of aliases
- [**atuin**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/atuin.plugins.bash): Load Atuin! The magical shell history (via ble.sh in bash)
- [**autojump**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/autojump.plugins.bash): Autojump configuration
- [**aws**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/aws.plugins.bash): AWS helper functions
- [**base**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/base.plugins.bash): Miscellaneous base tools
- [**basher**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/basher.plugins.bash): Initializes basher, the shell package manager
- [**blesh**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/blesh.plugins.bash): load ble.sh, the Bash line editor!
- [**brew**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/brew.plugins.bash): Load Homebrew shell environment and user-local binaries
- [**bun**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/bun.plugins.bash): Configure Bun install directory and add Bun to PATH
- [**cargo**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/cargo.plugins.bash): Configure Cargo install directory and add Cargo binaries to PATH
- [**chruby**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/chruby.plugins.bash): Load chruby (from the Homebrew prefix, `/usr/local` or `/usr`)
- [**cht-sh**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/cht-sh.plugins.bash): Simplify `curl cht.sh/<query>` to `cht.sh <query>`
- [**cmd-returned-notify**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/cmd-returned-notify.plugins.bash): Alert (BEL) when a command takes longer than a threshold to complete
- [**compress**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/compress.plugins.bash): Compression tools
- [**direnv**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/direnv.plugins.bash): Load [direnv](https://direnv.net/)
- [**dirs**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/dirs.plugins.bash): Directory stack navigation
- [**docker-compose**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/docker-compose.plugins.bash): Helper functions for using docker-compose
- [**docker**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/docker.plugins.bash): Helpers to more easily work with Docker
- [**edit-mode-emacs**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/edit-mode-emacs.plugins.bash): Enable emacs editing mode
- [**edit-mode-vi**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/edit-mode-vi.plugins.bash): Enable vi editing mode
- [**encode**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/encode.plugins.bash): Encoding and decoding functions
- [**explain**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/explain.plugins.bash): mankier.com explain function to explain other commands
- [**extract**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/extract.plugins.bash): Universal archive extractor
- [**fasd**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/fasd.plugins.bash): load fasd, if you are using it
- [**fzf**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/fzf.plugins.bash): Load fzf, a Command-line fuzzy finder written in Go
- [**gif**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/gif.plugins.bash): Video to gif, gif to WebM helper functions
- [**git-subrepo**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/git-subrepo.plugins.bash): Loads Git Submodule Alternative via git-subrepo and initialize completions
- [**git**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/git.plugins.bash): Git helper functions
- [**gitstatus**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/gitstatus.plugins.bash): Speeds up your life by using gitstatus for [git status](https://github.com/romkatv/gitstatus) calculations
- [**go**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/go.plugins.bash): Go environment variables & path configuration
- [**goenv**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/goenv.plugins.bash): Load Go version management via goenv
- [**gradle**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/gradle.plugins.bash): Add a gw command to use gradle wrapper if present, else use system gradle
- [**hg**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/hg.plugins.bash): Mercurial helper functions
- [**history**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/history.plugins.bash): history manipulation
- [**hub**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/hub.plugins.bash): Load GitHub support to git on the command-line via hub
- [**java**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/java.plugins.bash): Java and JAR helper functions
- [**jekyll**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/jekyll.plugins.bash): Manage your jekyll site
- [**jenv**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/jenv.plugins.bash): Manage your Java environment via jenv
- [**jgitflow**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/jgitflow.plugins.bash): Maven jgitflow build helpers (the branch pushed on finish is `JGITFLOW_MASTER_BRANCH`, `master` by default)
- [**json**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/json.plugins.bash): Syntax-highlight JSON strings or files
- [**jump**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/jump.plugins.bash): Initialize [jump](https://github.com/gsamokovarov/jump). Add `export JUMP_OPTS=("--bind=z")` to change keybinding
- [**latex**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/latex.plugins.bash): Add mactex to the path if its present
- [**man**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/man.plugins.bash): Colorize man pages for better readability
- [**nginx**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/nginx.plugins.bash): Manage your nginx service
- [**node**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/node.plugins.bash): Add the bin folder of a custom npm prefix to PATH
- [**nvm**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/nvm.plugins.bash): Node version manager configuration (lazy-loaded for fast startup)
- [**osx**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/osx.plugins.bash): OSX specific functions
- [**percol**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/percol.plugins.bash): Search & Select history with percol
- [**pipx**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/pipx.plugins.bash): Install and Run Python Applications in Isolated Environments
- [**postgres**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/postgres.plugins.bash): Postgres helper functions
- [**powerline**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/powerline.plugins.bash): Enables powerline daemon
- [**proxy**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/proxy.plugins.bash): Proxy Tools
- [**pyenv**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/pyenv.plugins.bash): Python version management via pyenv (lazy-loaded for fast startup)
- [**pygmentize**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/pygmentize.plugins.bash): pygmentize instead of cat to terminal if possible
- [**python**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/python.plugins.bash): Python helpers: `shttp` (http.server in the current directory) and `pyedit`
- [**rails**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/rails.plugins.bash): Helper functions for Ruby on Rails
- [**rbenv**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/rbenv.plugins.bash): Ruby Version Manager via rbenv
- [**ruby**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/ruby.plugins.bash): Ruby and rubygems specific functions and settings
- [**rvm**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/rvm.plugins.bash): Ruby Version Manager (RVM)
- [**sdkman**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/sdkman.plugins.bash): Load Software Development Kit Manager
- [**server**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/server.plugins.bash): Create Simple HTTP Server
- [**ssh**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/ssh.plugins.bash): SSH helper functions
- [**sshagent**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/sshagent.plugins.bash): SSH Agent helper functions
- [**subversion**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/subversion.plugins.bash): SVN helper functions
- [**sudo**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/sudo.plugins.bash): Toggle sudo with ESC-ESC
- [**thefuck**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/thefuck.plugins.bash): Initialization for fuck
- [**tmux**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/tmux.plugins.bash): Make sure that tmux is launched in 256 color mode
- [**tmuxinator**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/tmuxinator.plugins.bash): Sources tmuxinator script if available
- [**url**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/url.plugins.bash): Basic url handling and manipulation functions
- [**xterm**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/xterm.plugins.bash): Automatically set your xterm title with host and location info
- [**zoxide**](https://github.com/g-udi/gaudi-bash-plugins/blob/main/lib/zoxide.plugins.bash): Zoxide is a smarter cd command for your shell.

## Completions

- [**asdf**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/asdf.completions.bash): asdf completions | The multiple runtime version manager
- [**awscli**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/awscli.completions.bash): AWS CLI completions
- [**brew**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/brew.completions.bash): Brew completions
- [**bundler**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/bundler.completions.bash): Bash completion for the `bundle` command
- [**capistrano**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/capistrano.completions.bash): Bash completion support for Capistrano
- [**cargo**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/cargo.completions.bash): Cargo (Rust package manager) completion
- [**composer**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/composer.completions.bash): Composer completion
- [**conda**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/conda.completions.bash): Enable Conda completions
- [**consul**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/consul.completions.bash): Consul bash completions
- [**defaults**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/defaults.completions.bash): Bash command line completion for defaults
- [**dirs**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/dirs.completions.bash): Bash completion support for the 'dirs' plugin (commands G, R).
- [**docker-compose**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/docker-compose.completions.bash): Docker Compose bash completions
- [**docker**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/docker.completions.bash): Docker bash completions
- [**dotnet**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/dotnet.completions.bash): bash parameter completion for the dotnet CLI
- [**export**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/export.completions.bash): Export bash completions
- [**fabric**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/fabric.completions.bash): Fabric bash completions
- [**flutter**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/flutter.completions.bash): Flutter bash completions
- [**gaudi-bash**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/gaudi-bash.completions.bash): Gaudi bash completions (completes the `gbash` command)
- [**gcloud**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/gcloud.completions.bash): Google Cloud bash completions
- [**gem**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/gem.completions.bash): Gem bash completions
- [**git**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/git.completions.bash): Git bash completions
- [**git_flow**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/git_flow.completions.bash): Gitflow bash completions
- [**github-cli**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/github-cli.completions.bash): GitHub CLI completion
- [**go**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/go.completions.bash): Go bash completions
- [**gradle**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/gradle.completions.bash): Gradle bash completions
- [**grunt**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/grunt.completions.bash): Grunt bash completions
- [**gulp**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/gulp.completions.bash): Gulp bash completions
- [**helm**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/helm.completions.bash): Helm (Kubernetes Package Manager) completions
- [**homesick**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/homesick.completions.bash): Homesick bash completions
- [**kind**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/kind.completions.bash): kind (Kubernetes IN Docker) completion
- [**kubectl**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/kubectl.completions.bash): Kubectl (Kubernetes CLI) completion
- [**lerna**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/lerna.completions.bash): Lerna bash completions
- [**makefile**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/makefile.completions.bash): Makefile bash completions
- [**maven**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/maven.completions.bash): Maven bash completions
- [**minikube**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/minikube.completions.bash): Minikube (Local Kubernetes) completion
- [**ngrok**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/ngrok.completions.bash): Ngrok bash completions
- [**npm**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/npm.completions.bash): NPM (Node Package Manager) completion
- [**nvm**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/nvm.completions.bash): NVM (Node Version Manager) bash completions
- [**openshift**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/openshift.completions.bash): Openshift bash completions
- [**pack**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/pack.completions.bash): pack (Cloud Native Buildpacks CLI) completion
- [**packer**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/packer.completions.bash): Packer bash completions
- [**pip**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/pip.completions.bash): Python pip package manager bash completions
- [**pm2**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/pm2.completions.bash): PM2 bash completions
- [**rabbitmq**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/rabbitmq.completions.bash): RabbitMQ bash completions
- [**rake**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/rake.completions.bash): Bash completion support for Rake, Ruby Make
- [**rustup**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/rustup.completions.bash): Rustup (Rust toolchain installer) completion
- [**rvm**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/rvm.completions.bash): Bash completion support for RVM
- [**sdkman**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/sdkman.completions.bash): Sdkman bash completions
- [**sqlmap**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/sqlmap.completions.bash): SQL Map bash completions
- [**ssh**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/ssh.completions.bash): Bash completion support for SSH
- [**svn**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/svn.completions.bash): SVN bash completions
- [**system**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/system.completions.bash): Loads the systems Bash completion modules
- [**terraform**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/terraform.completions.bash): Terraform bash completions
- [**tmux**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/tmux.completions.bash): Tmux bash completions
- [**travis**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/travis.completions.bash): Travis bash completions
- [**vagrant**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/vagrant.completions.bash): Vagrant bash completions
- [**vault**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/vault.completions.bash): Vault bash completion
- [**virsh**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/virsh.completions.bash): Bash completion for virsh - main CLI of libvirt borrowed from https://github.com/LuyaoHuang/virsh-bash-completion
- [**virtualbox**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/virtualbox.completions.bash): Virtualbox completions
- [**vuejs**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/vuejs.completions.bash): Vue.js completions
- [**yarn**](https://github.com/g-udi/gaudi-bash-completions/blob/main/lib/yarn.completions.bash): YARN CLI completions

## Aliases

- [**ag**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/ag.aliases.bash): The Silver Searcher (ag) aliases
- [**ansible**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/ansible.aliases.bash): Ansible abbreviations
- [**apt**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/apt.aliases.bash): apt and dpkg aliases for Ubuntu and Debian distros.
- [**bundler**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/bundler.aliases.bash): Ruby Bundler
- [**clipboard**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/clipboard.aliases.bash): pbcopy and pbpaste shortcuts to linux
- [**composer**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/composer.aliases.bash): Common composer abbreviations
- [**curl**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/curl.aliases.bash): Curl aliases for convenience.
- [**directory**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/directory.aliases.bash): Shortcuts for directory commands: ls, cd, &c.
- [**docker-compose**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/docker-compose.aliases.bash): docker compose abbreviations
- [**docker**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/docker.aliases.bash): Docker abbreviations/aliases
- [**emacs**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/emacs.aliases.bash): Emacs editor
- [**fuck**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/fuck.aliases.bash): fuck/please to retry last command with sudo
- [**gaudi-bash**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/gaudi-bash.aliases.bash): Aliases for common misspellings of the `gbash` command (`gbsh`, `gbahs`)
- [**general**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/general.aliases.bash): General aliases: sudo, grep, navigation (cd, ..), directory creation, history, etc.
- [**git**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/git.aliases.bash): Common git abbreviations
- [**gitsvn**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/gitsvn.aliases.bash): Common git-svn abbreviations
- [**gls**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/gls.aliases.bash): gls enhanced ls (port from Linux using coreutils)
- [**heroku**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/heroku.aliases.bash): Heroku task abbreviations
- [**hg**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/hg.aliases.bash): Mercurial abbreviations
- [**homebrew**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/homebrew.aliases.bash): Homebrew abbreviations
- [**homesick**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/homesick.aliases.bash): Homesick aliases
- [**kubectl**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/kubectl.aliases.bash): kubectl aliases
- [**ls**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/ls.aliases.bash): ls aliases to add coloring and extra arguments
- [**maven**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/maven.aliases.bash): Maven abbreviations
- [**node**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/node.aliases.bash): Node.js environment aliases
- [**npm**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/npm.aliases.bash): Common npm abbreviations
- [**osx**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/osx.aliases.bash): OSX specific aliases
- [**puppet**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/puppet.aliases.bash): Puppet aliases
- [**rails**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/rails.aliases.bash): Ruby Rails aliases
- [**svn**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/svn.aliases.bash): Common svn aliases
- [**systemd**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/systemd.aliases.bash): systemd service aliases
- [**terraform**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/terraform.aliases.bash): Aliases for Terraform/OpenTofu
- [**textmate**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/textmate.aliases.bash): textmate aliases
- [**tmux**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/tmux.aliases.bash): tmux terminal multiplexer aliases
- [**uuidgen**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/uuidgen.aliases.bash): uuidgen aliases
- [**vagrant**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/vagrant.aliases.bash): Vagrant aliases
- [**vault**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/vault.aliases.bash): Vault aliases
- [**vim**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/vim.aliases.bash): vim abbreviations
- [**yarn**](https://github.com/g-udi/gaudi-bash-aliases/blob/main/lib/yarn.aliases.bash): Yarn package manager aliases
