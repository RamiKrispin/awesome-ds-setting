Hello 👋 

After setting/reinstalling a couple of machines from scratch in the last few months, I decided for once and for all to document my default data science setting and environment.

I am working with macOS Montarey but most of this tools in this document are OS agnostic (e.g., Windows, Linux, etc.).

💡 **A pro tip** 👉🏼 avoid dropping a cup of ☕️ on your machine 🤦🏻‍♂️

This document covers:
- [Setting Git and SSH](https://github.com/RamiKrispin/awesome-ds-setting/blob/main/README.md#setting-git-and-ssh)
- [Install Command Lines Tools](https://github.com/RamiKrispin/awesome-ds-setting/blob/main/README.md#install-command-lines-tools)
- [Install R and RStudio](https://github.com/RamiKrispin/awesome-ds-setting/blob/main/README.md#install-r-and-rstudio)
- [Shortcuts](https://github.com/RamiKrispin/awesome-ds-setting/blob/main/README.md#shortcuts)


### Setting Git and SSH

Initial setting for Git

Source: https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup


**Note:** you may have to install `command line developer tools`, if it is not already installed on your machine, it will pop-up the following option, and you will have to install it:

![image](https://user-images.githubusercontent.com/12760966/139562122-8a360563-89f0-4108-b916-ca20a4be7fe1.png)


#### Set Git global options
##### Set users

``` shell
git config --global user.name "RamiKrispin"
git config --global user.email myemail@example.com
```

##### Set default branch name

The `init.defaultBranch` argument set the default branch name when running `git init`:

``` shell
git config --global init.defaultBranch main
```

##### Set global Git ignore file

Setting a global `gitignore` file will enable you to set general ignore roles that will apply to all the repositories in your machine.

``` shell
touch ~/.gitignore
git config --global core.excludesFile ~/.gitignore
```

Once the global Git ignore file is set, you just need to update the files you wish Git to ignore across all your local repositories. For example, the following will add `.DS_Store` to the global ignroe list:

``` shell 
echo .DS_Store >> ~/.gitignore
```

##### Set default editor

Git enables you to set the default shell code editor to create and edit your commit and tag messages with the `core.editor` argument:

``` shell
git config --global core.editor "vim"
```



#### Set SSH

Setting `SSH` key required to sync your local git repositories with the `origin`. To set SSH key on your local machine you need to use `ssh-keyget`:

``` shell
ssh-keygen -t ed25519 -C "YOUR_EAMIL@example.com"
```

Note that `-t` argument enable you to define the type of algorithm for authentication key, in this case I used `ed25519` and the `-C` argument used to add comment,in this case the user name email.

You will have to enter the file name for the SSH key, by default it will save it under the `~/.ssh` folder. 

**Note:** this process will generate two files:
- `your_ssh_key` is the private key, you should not expose it
- `your_ssh_key.pub` is the public key which will be used to to set the SSH on Github

The next step is to register the key on your Github account. Under setting select on the main menu `SSH and GPG keys` and click on the `New SSH key`:

<img width="1054" alt="Screen Shot 2021-10-30 at 4 57 58 PM" src="https://user-images.githubusercontent.com/12760966/139561558-66920a3a-7890-4247-887c-c94319100156.png">


And then, set the key name and paste the public key inside the key box:

![image](https://user-images.githubusercontent.com/12760966/139561686-f2bc745c-b112-46e3-be4c-07136080a4cf.png)


Next step is to update the `config` file on the `~/.ssh` folder. You can edit the `config` file with `vim`:

``` shell
vim ~/.ssh/config 
```

and add somewhere on the file the following code:

``` shell
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/your_ssh_key
```
Where `your_ssh_key` is the private key file name


Last, run the following to load the key:
```
ssh-add -K ~/.ssh/your_ssh_key
```


#### Resources

- Github documentation - https://docs.github.com/en/enterprise-server@3.0/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account
- `ssh-keyget` arguments -  https://www.ssh.com/academy/ssh/keygen
- A great video toturial about setting SSH:  https://www.youtube.com/watch?v=RGOj5yH7evk&t=1230s&ab_channel=freeCodeCamp.org
- Setting Git ignore - https://www.atlassian.com/git/tutorials/saving-changes/gitignore

### Install Command Lines Tools

This section covers core command lines tools.

#### Homebrew

The Homebrew (or `brew`) enables you to install CL packages and tools for Mac. To install `brew` run from the terminal:

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

More info available: https://brew.sh/

#### jq

The `jq` is a lightweight and flexible command-line JSON processor. You can install it with `brew`:

```shell
brew install jq
```

### Install Docker

There are multiple ways to spin a VM locally to run Docker. I typically use [Docker Desktop](https://www.docker.com/products/docker-desktop), and for learning purposes (e.g., Kubernetes) I also install [Minikube](https://minikube.sigs.k8s.io/docs/).

#### Install Docker Desktop

Go to [Docker website](https://docs.docker.com/get-docker/) and follow the intallation instractions according to your OS

#### Install Minikube

Minikube enables you to set virtual environment to run Docker. This is mainly relevant if you are using macOS or Windows and want to run Docker via cli. To install Minikube you will need to install first [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/), [hyperkit](https://github.com/moby/hyperkit). We will use `brew` to install all those components:

``` shell
brew install kubectl
brew install hyperkit
brew install docker
brew install minikube
```

Lunching minikube with the `start` argument and setting the memory and cpu allocation:

``` shell
> minikube start --memory 4096 --cpus 2 --driver hyperkit
😄  minikube v1.24.0 on Darwin 12.0.1
    ▪ MINIKUBE_ACTIVE_DOCKERD=minikube
✨  Using the hyperkit driver based on user configuration
👍  Starting control plane node minikube in cluster minikube
🔥  Creating hyperkit VM (CPUs=2, Memory=4096MB, Disk=20000MB) ...
🐳  Preparing Kubernetes v1.22.3 on Docker 20.10.8 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

Lunch Docker:

``` shell
eval $(minikube -p minikube docker-env)
```

Check the Docker status:
``` shell
> docker info
Client:
 Context:    default
 Debug Mode: false

Server:
 Containers: 15
  Running: 14
  Paused: 0
  Stopped: 1
 Images: 10
 Server Version: 20.10.8
 Storage Driver: overlay2
  Backing Filesystem: extfs
  Supports d_type: true
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: systemd
 Cgroup Version: 1
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 io.containerd.runtime.v1.linux runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: e25210fe30a0a703442421b0f60afac609f950a3
 runc version: 4144b63817ebcc5b358fc2c8ef95f7cddd709aa7
 init version: de40ad0
 Security Options:
  seccomp
   Profile: default
 Kernel Version: 4.19.202
 Operating System: Buildroot 2021.02.4
 OSType: linux
 Architecture: x86_64
 CPUs: 2
 Total Memory: 3.847GiB
 Name: minikube
 ID: 2IME:DJBF:L32S:HA4Q:DFCX:2LRI:JBCQ:6ORQ:RHUE:Q4S6:7WYE:PUD7
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Registry: https://index.docker.io/v1/
 Labels:
  provider=hyperkit
 Experimental: false
 Live Restore Enabled: false
 Product License: Community Engine
```



#### Resources 
- Minikube documentation - https://minikube.sigs.k8s.io/docs/start/
- Installation guide - https://www.youtube.com/watch?v=zwmjzU62LWQ&ab_channel=AutomationStepbyStep
- Kubectl - https://kubernetes.io/docs/reference/kubectl/overview/
- hyperkit - https://github.com/moby/hyperkit

### Install R and RStudio

To set in your machine R and RStudio you should start first with installing R from CRAN. Go to https://cran.r-project.org/ and select `Download R for macOS` and select the release you wish to install and download. Once you finish to download the build you select open the `pkg` fild and start to install it:

![image](https://user-images.githubusercontent.com/12760966/140316216-0e51e0b1-4f32-4a63-8913-160fb939895a.png)


**Note:** Older releases available on [CRAN Archive](https://cran-archive.r-project.org/bin/macosx/).

Once R installed, you can install RStudio - go to https://www.rstudio.com/products/rstudio/download/ and select the version and download it:
![image](https://user-images.githubusercontent.com/12760966/140406353-912757a3-542c-46b5-92cb-c8a6803c2d69.png)

Once finish to download it move the application into the Application folder.

#### Set RStudio

- Workspace - select `Never` to `Save workspace to .RData on exit` option
- History - go to `Tools` -> `Global Option` -> `General` and untick the first options - `Always save history...`. This will avoid saving the session on quit.
- Code: 
  - Code snippet - go to `Tools` -> `Global Option` -> `Code`, on the Snippet menue tick the `Enable code snippets` option and select `Edit Snippets` button to edit your snippits. My default snippets available [here](https://gist.github.com/RamiKrispin/b16b63688746c4cfd01ec21cc7c25d2a).
  - Rainbow parentheses 🌈 - `Tools` -> `Global Option` -> `Code`, select the `Display` tab and tick the `Rainbow parentheses` box.
- Appearance - go to `Tools` -> `Global Option` -> `Appearance` and select the font type and size, and editor theme (Merbivore Soft):

![image](https://user-images.githubusercontent.com/12760966/142575001-0cc0d549-25c6-4eab-9d1a-3f540d6ba8ae.png)


#### RStudio main shortcuts

- Clear console - `Ctrl` +  `L`
- Clost current document - `Cmd` + `W`
- Move focus to the Source panel - `Cmd` + `1`
- Move focus to the Console panel - `Cmd` + `2`
- Move tab left - `Cmd` + `]`
- Move tab right - `Cmd` + `[`
- Move tab to first - `Cmd` + `P`
- Move tab to last - `Cmd` + `\`
- New Rmarkdown notebook - `Cmd` + `R`

#### Install XQuartz

The XQuartz is an open-source project that provides required for graphic applications (X11) for macOS (similar to the X.Org X Window System functionality). To install it go to https://www.xquartz.org/ - download and install it.

### Shortcuts

This section covers the installation and setting of additional tools and features such as screen spliting, shortcuts, etc.

#### Spectacle

Spectacle is great tool for moving and resizing your windows. To install it go to https://www.spectacleapp.com/ and download it. Once installed you can modify the default setting:

![image](https://user-images.githubusercontent.com/12760966/140613227-e14dc19b-ba3e-435f-b67f-4f12bcc28c27.png)

**Note:** Once installed, don't forget to select the checkbox `Launch Spectacle at login` to keep the the app active after reseting/shoutdown your machine.

#### Keyboard Shortcuts

* Change language - if you are using more than one language, you can add a keyboard shortcut for switching between them. Go to `System Preferences...` -> `keyboard` and select the shortcut tab. Under the `Input Sources` tick the `Select the previous input source option`:

![image](https://user-images.githubusercontent.com/12760966/140625490-3b688dee-fc09-4252-9626-05d10094187e.png)

**Note:** that you can modify the keyboard shortcut by clicking shortcut definition in that row


