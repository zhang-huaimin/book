# rust
## 源配置
### 国内源
* 字节跳动
```bash
wget https://rsproxy.cn/rustup-init.sh
chmod +x rustup-init.sh
./rustup-init.sh -y
mkdir -p ~/.cargo && . "$HOME/.cargo/env"
cargo install mcfly && echo 'eval "$(mcfly init bash)"' >> ~/.bashrc
cargo install gitui cross bottom
cargo install ripgrep && cargo install fd-find
cargo install zoxide && echo 'eval "$(zoxide init bash)"' >> ~/.bashrc && echo 'alias cd=z' >> ~/.bashrc
```