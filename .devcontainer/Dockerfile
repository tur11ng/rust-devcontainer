# .devcontainer/Dockerfile

# Use a specific Ubuntu version for consistency
FROM ubuntu:24.04

# Set environment variables to non-interactive to avoid prompts during package installations
ENV DEBIAN_FRONTEND=noninteractive

# Install system packages
RUN apt update && \
    apt install --yes \
        build-essential \
        git \
        llvm \
        clang \
        libclang-dev \
        lld \
        gdb \
        clangd \
        curl \
        mingw-w64 \
        zsh \
        fonts-powerline && \
    apt clean && \
    rm -rf /var/lib/apt/lists/*

# Install Rust using rustup
RUN curl https://sh.rustup.rs -sSf | sh -s -- --default-toolchain stable -y

# Add Cargo's bin directory to PATH
ENV PATH=/root/.cargo/bin:$PATH

# Configure Rust components
RUN rustup override set stable && \
    rustup component add rust-src rustfmt clippy && \
    rustup target add x86_64-pc-windows-gnu && \
    cargo install --locked bindgen-cli

# Set the working directory
WORKDIR /workspaces

# Install Oh My Zsh and plugins
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" && \
    git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions && \
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

# Change the default shell to Zsh for the root user
RUN chsh -s $(which zsh)

# Configure Oh My Zsh
RUN sed -i 's/ZSH_THEME="robbyrussell"/ZSH_THEME="agnoster"/' ~/.zshrc && \
    sed -i 's/plugins=(git)/plugins=(git zsh-autosuggestions zsh-syntax-highlighting)/' ~/.zshrc