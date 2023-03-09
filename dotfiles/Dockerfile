FROM ubuntu:latest

# Install updates and some basic packages
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y curl wget git

# Install zsh
RUN apt-get install -y zsh tmux stow vim

WORKDIR /root
RUN git clone https://github.com/vuhailongkl97/dotfiles

RUN sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
RUN rm /root/.zshrc

# Install VimPlug
RUN sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'

# Install nvim
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
RUN . ~/.bashrc && nvm install node
RUN apt install pip locales -y
RUN pip install pynvim --upgrade
# Uncomment the line for the UTF-8 locale in the locale.gen file
RUN sed -i 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen

# Generate the updated locale settings
RUN locale-gen en_US.UTF-8

# Set the LANG environment variable to the UTF-8 locale
ENV LANG en_US.UTF-8

RUN curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim.appimage
RUN chmod u+x nvim.appimage
RUN ./nvim.appimage --appimage-extract
RUN ./squashfs-root/AppRun --version
RUN mv squashfs-root /
RUN ln -s /squashfs-root/AppRun /usr/bin/nvim


# custome zsh
RUN git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
RUN git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
RUN echo "source ~/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
#RUN nvim +PlugInstall +qall
# for c/c++ dev
RUN apt install ccls clang-format -y

# Remove zsh-syntax-highlighting.zsh isn't suitable for this
RUN sed -i '/^source \//d' ~/.zshrc

# Setup zsh as default bash for tmux
RUN echo 'set-option -g default-shell /usr/bin/zsh' >> ~/.tmux.conf

# Need restow here
RUN rm .zshrc .tmux.conf
RUN cd dotfiles && stow -R --adopt .
RUN nvim --headless +'PlugInstall --sync' +qa

# Install lazygit
RUN LAZYGIT_VERSION=$(curl -s "https://api.github.com/repos/jesseduffield/lazygit/releases/latest" | grep -Po '"tag_name": "v\K[^"]*'); curl -Lo lazygit.tar.gz "https://github.com/jesseduffield/lazygit/releases/latest/download/lazygit_${LAZYGIT_VERSION}_Linux_x86_64.tar.gz"
RUN tar xf lazygit.tar.gz lazygit
RUN mv lazygit /usr/bin/lg

RUN apt install universal-ctags ripgrep -y

# Set the entrypoint to a bash shell
ENTRYPOINT ["/bin/bash"]
