FROM r-base:latest

# System libraries needed for your packages
RUN apt-get update && apt-get install -y \
    git \
    build-essential \
    libcurl4-openssl-dev \
    libssl-dev \
    libxml2-dev \
    libicu-dev \
    libxt-dev \
    && apt-get clean

# Install remotes so we can install specific versions from CRAN archives
RUN R -q -e "install.packages('remotes', repos='https://cloud.r-project.org')"

# Install specific versions
RUN R -q -e "\
  remotes::install_version('ps', version = '1.9.1'); \
  remotes::install_version('evaluate', version = '1.0.5'); \
  remotes::install_version('highr', version = '0.11'); \
  remotes::install_version('xfun', version = '0.54'); \
  remotes::install_version('yaml', version = '2.3.10'); \
  remotes::install_version('lazyeval', version = '0.2.2'); \
  remotes::install_version('lifecycle', version = '1.0.4'); \
  remotes::install_version('pkgbuild', version = '1.4.8'); \
  remotes::install_version('R.methodsS3', version = '1.8.2'); \
  remotes::install_version('R.oo', version = '1.27.1'); \
  remotes::install_version('R.utils', version = '2.13.0'); \
  remotes::install_version('processx', version = '3.8.6'); \
  remotes::install_version('backports', version = '1.5.0'); \
  remotes::install_version('cli', version = '3.6.5'); \
  remotes::install_version('digest', version = '0.6.39'); \
  remotes::install_version('glue', version = '1.8.0'); \
  remotes::install_version('knitr', version = '1.50'); \
  remotes::install_version('rex', version = '1.2.1'); \
  remotes::install_version('brew', version = '1.0-10'); \
  remotes::install_version('commonmark', version = '2.0.0'); \
  remotes::install_version('desc', version = '1.4.3'); \
  remotes::install_version('pkgload', version = '1.4.1'); \
  remotes::install_version('purrr', version = '1.2.0'); \
  remotes::install_version('rlang', version = '1.1.6'); \
  remotes::install_version('stringr', version = '1.6.0'); \
  remotes::install_version('withr', version = '3.0.2'); \
  remotes::install_version('cpp11', version = '0.5.2'); \
  remotes::install_version('magrittr', version = '2.0.4'); \
  remotes::install_version('R.cache', version = '0.17.0'); \
  remotes::install_version('rprojroot', version = '2.1.1'); \
  remotes::install_version('vctrs', version = '0.6.5'); \
  remotes::install_version('callr', version = '3.7.6'); \
  remotes::install_version('collections', version = '0.3.9'); \
  remotes::install_version('fs', version = '1.6.6'); \
  remotes::install_version('jsonlite', version = '2.0.0'); \
  remotes::install_version('lintr', version = '3.2.0'); \
  remotes::install_version('R6', version = '2.6.1'); \
  remotes::install_version('roxygen2', version = '7.3.3'); \
  remotes::install_version('stringi', version = '1.8.7'); \
  remotes::install_version('styler', version = '1.11.0'); \
  remotes::install_version('xml2', version = '1.5.0'); \
  remotes::install_version('xmlparsedata', version = '1.0.5'); \
  remotes::install_version('languageserver', version = '0.3.16'); \
"

# Create vscode user with UID/GID and bash
RUN mkdir -p /workspace 
RUN chown -R docker:$docker /workspace

WORKDIR /workspace
SHELL ["/bin/bash", "-c"]

# Add colorful, git-aware PS1
RUN echo -e 'parse_git_branch() {\n\
branch=$(git branch --show-current 2>/dev/null)\n\
if [ -n "$branch" ]; then echo " (\e[0;33m$branch\e[0m)"; fi\n\
}\n\
export PS1="\[\e[0;32m\]\u@\h\[\e[0m\]->\[\e[0;34m\]\w\[\e[0m\]\$(parse_git_branch) \$ "' >> /home/docker/.bashrc

USER docker