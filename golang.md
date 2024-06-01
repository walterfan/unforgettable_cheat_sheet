# Golang Cheat Sheet

<!-- TOC -->
- [Golang Cheat Sheet](#golang-cheat-sheet)
	- [Fundarmental](#fundarmental)
		- [Golang Installation](#golang-installation)

<!-- /TOC -->


## Fundarmental
- No classes, but structs with methods
- Interfaces
- No implementation inheritance. There's type embedding, though.
- Functions are first class citizens
- Functions can return multiple values
- Has closures
- Pointers, but not pointer arithmetic
- Built-in concurrency primitives: Goroutines and Channels

### Golang Installation

#### Single version

Linux
```
# download go
https://github.com/golang/go/releases
### STANDARD WAY
sudo tar -C /usr/local -xzf go$VERSION.$OS-$ARCH.tar.gz
# set PATH
# Add /usr/local/go/bin to PATH with /etc/profile or $HOME/.profile
export GO_HOME=/usr/local/go
export PATH=$PATH:$GO_HOME/bin
```
MacOS
```
# (1) install go by brew
brew install go
# or
brew upgrade go

# (2) Env variables
# /etc/profile or $HOME/.profile
export GOPATH=$HOME
export PATH=$PATH:$GOPATH/bin
```