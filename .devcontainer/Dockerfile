FROM r-base:latest

###############################
# System dependencies
###############################
RUN apt-get update && apt-get install -y \
    git=1:2.51.0-1 \
    build-essential=12.12 \
    libcurl4-openssl-dev=8.17.0-2 \
    libssl-dev=3.5.4-1 \
    libxml2-dev=2.15.1+dfsg-0.4 \
    libicu-dev=76.1-4 \
    libxt-dev=1:1.2.1-1.3 \
    curl=8.17.0-2 \
    bash=5.3-1 \
    ca-certificates=20250419 \
    gnupg=2.4.8-4 \
    openssh-client=1:10.2p1-2 \
    ruby-full=1:3.3 \
    zlib1g-dev=1:1.3.dfsg+really1.3.1-1+b1 \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

###############################
# Ensure global R library
###############################
RUN mkdir -p /usr/local/lib/R/site-library && \
    chmod -R 777 /usr/local/lib/R/site-library

###############################
# Install remotes
###############################
RUN R -q -e "install.packages('remotes', repos='https://cloud.r-project.org')"

###############################
# Install specific R packages
###############################
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
  remotes::install_version('checkpoint', version = '0.4.10'); \
  remotes::install_version('fastmap', version = '1.2.0'); \
  remotes::install_version('bit', version = '4.6.0'); \
  remotes::install_version('cachem', version = '1.1.0'); \
  remotes::install_version('bit64', version = '4.6.0-1'); \
  remotes::install_version('blob', version = '1.2.4'); \
  remotes::install_version('DBI', version = '1.2.3'); \
  remotes::install_version('memoise', version = '2.0.1'); \
  remotes::install_version('pkgconfig', version = '2.0.3'); \
  remotes::install_version('plogr', version = '0.2.0'); \
  remotes::install_version('Rcpp', version = '1.1.0'); \
  remotes::install_version('data.table', version = '1.17.8'); \
  remotes::install_version('RSQLite', version = '2.4.4'); \
  remotes::install_version('RcppEigen', version = '0.3.4.0.2'); \
  remotes::install_version('qtl2', version = '0.38'); \
"

###############################
# Install GitHub CLI
###############################
RUN mkdir -p /etc/apt/keyrings && \
    curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | \
        gpg --dearmor -o /etc/apt/keyrings/githubcli-archive-keyring.gpg && \
    chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" \
        > /etc/apt/sources.list.d/github-cli.list && \
    apt-get update && apt-get install -y gh && \
    rm -rf /var/lib/apt/lists/*

###############################
# Create docker user
###############################
RUN usermod -d /home/docker docker || true
RUN mkdir -p /home/docker && chown -R docker:docker /home/docker
ENV HOME=/home/docker

###############################
# Workspace setup
###############################
RUN mkdir -p /workspace && chown -R docker:docker /workspace
WORKDIR /workspace

###############################
# Add colored Git PS1
###############################
RUN printf '\n\
parse_git_branch() {\n\
  branch=$(git branch --show-current 2>/dev/null)\n\
  if [ -n "$branch" ]; then echo -e " (\\e[0;33m$branch\\e[0m)"; fi\n\
}\n\
export PS1="\\[\\e[0;32m\\]\\u@\\h\\[\\e[0m\\]->\\[\\e[0;34m\\]\\w\\[\\e[0m\\]\\$(parse_git_branch) $ "' >> /home/docker/.bashrc

###############################
# Install Jekyll and Bundler
###############################
RUN gem install jekyll bundler


USER docker

