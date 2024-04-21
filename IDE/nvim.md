1. **Install neovim:** The first step is to install neovim, for this we will use the github guide of [neovim](https://github.com/neovim/neovim/blob/master/INSTALL.md)

For X86_64:

```bash
curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux64.tar.gz
sudo rm -rf /opt/nvim
sudo tar -C /opt -xzf nvim-linux64.tar.gz
```

After this step add this to `~/.bashrc`:
```
export PATH="$PATH:/opt/nvim-linux64/bin"
```

After that you need to re-login or just run the following command:

```bash
source ~/.bashrc
```

2. **Install nvchad:** To install nvchad we also use their official [documentation](https://nvchad.com/docs/quickstart/install)

```bash
git clone https://github.com/NvChad/starter ~/.config/nvim && nvim
```

after that the nvim will open automatically and run the instalation process, if not run the following vim command: 
```
:MasonInstallAll
```



