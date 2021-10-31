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


Resources

- Github documentation - https://docs.github.com/en/enterprise-server@3.0/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account
- `ssh-keyget` arguments -  https://www.ssh.com/academy/ssh/keygen
- A great video toturial about setting SSH:  https://www.youtube.com/watch?v=RGOj5yH7evk&t=1230s&ab_channel=freeCodeCamp.org

