FROM fedora:37

#   Switch to a temporary location to avoid
# unnecessary  storage usage later.
WORKDIR /tmp

# Update & install whatever is needed
RUN dnf update -y
RUN dnf install -y git wget time which ca-certificates zsh gcc make util-linux-user sudo npm ncurses-devel npm pip

# Install Prettier for HTML/CSS formatting
RUN npm install --save-dev --save-exact prettier

# Grab Vim fron source instead and compile it with Python3 support
RUN git clone https://github.com/vim/vim.git && cd vim && ./configure --enable-python3interp=yes --with-python3-config-dir=/usr/bin/python3.11/config-* && make && make install

###############
# SET-UP USER #
###############
RUN useradd archie

RUN usermod -aG wheel archie

USER archie

WORKDIR /home/archie

#############
# PIP STUFF #
#############
#RUN pip3 install --user --upgrade pip

#ADD ./requirements.txt /app/requirements.txt
#RUN pip3 install --user -r /app/requirements.txt

#################
# SET-UP PREZTO #
#################
USER root

# Switch the default shell to Zshell
RUN chsh -s "$(which zsh)"

# Install Zprezto
COPY init_zprezto.sh .

RUN chmod +x init_zprezto.sh

USER archie

RUN rm /home/archie/.zshrc

RUN ./init_zprezto.sh

##################
# SET-UP ALIASES #
##################
#   I need the rich/colourful environment experience
# Plus, it possibly solves a Prompt.app issue on iOS/iPadOS
ENV TERM="screen-256color"

# Basic aliases to avoid minor inconviences
RUN echo 'alias pip=pip3' >> ~/.zshrc
RUN echo 'alias python=python3' >> ~/.zshrc

#########
# OTHER #
#########
# Grab my Vim config
RUN git clone --recursive https://gitlab.cansurmeli.com/can/vim-config /home/$(whoami)/.vim

