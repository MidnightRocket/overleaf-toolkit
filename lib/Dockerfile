FROM docker.io/sharelatex/sharelatex:latest


RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
	--mount=type=cache,target=/var/lib/apt,sharing=locked \
	export DEBIAN_FRONTEND=noninteractive DEBCONF_NONINTERACTIVE_SEEN=true \
	&& apt-get -q update \
	&& apt-get -q dist-upgrade -y \
	&& apt-get -q --no-install-recommends install -y \
		inkscape \
		lilypond \
		xzdec \
	&& apt-get -q autoremove -y


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





# Have to use latest version of pygments pip. Not available via apt
RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
	--mount=type=cache,target=/var/lib/apt,sharing=locked \
	export DEBIAN_FRONTEND=noninteractive DEBCONF_NONINTERACTIVE_SEEN=true \
	&& apt-get -q update \
	&& apt-get -q --no-install-recommends install -y \
		# python-full \
		python3-venv \
		# python3-pip \
	&& apt-get -q autoremove -y

RUN python3 -m venv /opt/venv/

ENV PATH="/opt/venv/bin:$PATH"

RUN pip install --upgrade --no-cache-dir pygments




# RUN pipx install --global --pip-args="--no-cache-dir" pygments


# To allow minted to work properly
# https://github.com/rigon/docker-sharelatex-full/blob/4a2d897b13646aa2ce42f8a8442b2d84815e3810/Dockerfile#L42-L44
# https://github.com/tuetenk0pp/sharelatex-full/blob/825a5f72a0192bb2050fc475129e7ee14d78e342/Dockerfile#L48-L50
RUN TEXLIVE_FOLDER=$(find /usr/local/texlive/ -type d -name '20*') \
	&& echo '% enable shell-escape by default' >> "/$TEXLIVE_FOLDER/texmf.cnf" \
	&& echo 'shell_escape = t' >> "/$TEXLIVE_FOLDER/texmf.cnf"
