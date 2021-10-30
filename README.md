### Setting Git and Github 

Initial setting for Git

Source: https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup

#### Set users

``` shell
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
```

#### Set default branch name

The `init.defaultBranch` argument set the default branch name when running `git init`:

``` shell
git config --global init.defaultBranch main
```

#### Set SSH

Setting `SSH` key required to sync your local git repositories with the `origin`. To set SSH key on your local machine you need to use `ssh-keyget`:

``` shell
ssh-keygen -t ed25519 -C "YOUR_EAMIL@example.com"
```

Note that `-t` argument enable you to define the type of algorithm for authentication key, in this case I used `ed25519` and the `-C` argument used to add comment,in this case the user name email.

You will have to ender that file name for the SSH key, by default it will save it under the `~/.ssh` folder.



``` shell
vim ~/.ssh/config 

Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_rsa_personal


ssh-add -K ~/.ssh/id_rsa_personal

```

Resources

- Github documentation - https://docs.github.com/en/enterprise-server@3.0/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account
- `ssh-keyget` arguments -  https://www.ssh.com/academy/ssh/keygen
- A great video toturial about setting SSH:  https://www.youtube.com/watch?v=RGOj5yH7evk&t=1230s&ab_channel=freeCodeCamp.org

