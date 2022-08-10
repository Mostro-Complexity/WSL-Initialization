
FROM dorowu/ubuntu-desktop-lxde-vnc:focal-lxqt

COPY ./sources.list /etc/apt/sources.list

RUN gpg --keyserver keyserver.ubuntu.com --recv 4EB27DB2A3B88B8B \
    && gpg --export --armor 4EB27DB2A3B88B8B | apt-key add - \
    && wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add - \
    && add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" \
    && apt-get update \
    && apt-get -y install zsh git vim code \
    && apt autoclean -y \
    && apt autoremove -y \
    && rm -rf /var/lib/apt/lists/* \
    && chsh -s /bin/zsh \
    && wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh \
    && bash ./install.sh \
    && git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions \
    && git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

WORKDIR /root
RUN wget https://github.com/projectchrono/chrono/archive/refs/tags/7.0.3.tar.gz 

COPY ./.zshrc /root/.zshrc
COPY ./robbyrussell.zsh-theme /root/.oh-my-zsh/themes/robbyrussell.zsh-theme

ENV PATH=/root/miniconda/bin:$PATH

RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
    && bash Miniconda3-latest-Linux-x86_64.sh -b -p /root/miniconda \
    && rm Miniconda3-latest-Linux-x86_64.sh \
    && /root/miniconda/bin/conda clean -tipsy \
    && /root/miniconda/bin/conda init zsh \
    && conda install python=3.10 \
    && conda config --add channels https://conda.anaconda.org/conda-forge \
    && conda config --add channels https://conda.anaconda.org/intel \
    && conda install -c anaconda numpy gevent flask gevent-websocket \
    && conda install -c conda-forge irrlicht \
    && conda install -c dlr-sc opencascade \
    && conda install -c projectchrono pychrono

