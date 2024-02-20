FROM docker.io/sharelatex/sharelatex:latest
RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
    export DEBIAN_FRONTEND=noninteractive DEBCONF_NONINTERACTIVE_SEEN=true \
    && apt-get -q update \
    && apt-get -q dist-upgrade -y \
    && apt-get -q install -y \
        python3-pip \
        inkscape \
        lilypond \
        xzdec \
    && apt-get -q autoremove -y \
    && apt-get -q clean -y \
    && rm -rf /var/lib/apt/lists/* \
    && pip install --no-cache-dir --upgrade pip \
    && pip install --no-cache-dir \
        pygments
        # Have to use latest version of pygments. Not available via apt


# https://github.com/tuetenk0pp/sharelatex-full
# update tlmgr itself
RUN wget "https://mirror.ctan.org/systems/texlive/tlnet/update-tlmgr-latest.sh" \
    && sh update-tlmgr-latest.sh \
    && rm update-tlmgr-latest.sh \
    && tlmgr --version \
    && tlmgr update --self --all \
    && tlmgr path add \
    && tlmgr install scheme-full \
    && tlmgr path add

# To allow minted to work properly
# https://github.com/rigon/docker-sharelatex-full/blob/4a2d897b13646aa2ce42f8a8442b2d84815e3810/Dockerfile#L42-L44
# https://github.com/tuetenk0pp/sharelatex-full/blob/825a5f72a0192bb2050fc475129e7ee14d78e342/Dockerfile#L48-L50
RUN TEXLIVE_FOLDER=$(find /usr/local/texlive/ -type d -name '20*') \
    && echo '% enable shell-escape by default' >> "/$TEXLIVE_FOLDER/texmf.cnf" \
    && echo 'shell_escape = t' >> "/$TEXLIVE_FOLDER/texmf.cnf"
