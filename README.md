# Modular Todo
A toy project to demonstrate language, technology, and systems knowledge.

## Architecture
Funtionally, this is a dead-simple "Todo" app... the twist is that each logical component of the software is shipped as a collection of modules that are hot-swappable _at runtime_.

The breakdown is conventional:
```
Front End <--> Back End <--> Database
```

However, each of the three segments has been built several different times using different languages, frameworks, and technologies for each module.

| Front Ends | Back Ends | Databases |
|---|---|---|
| CLI | Golang | PostgreSQL |
| React | Java | MongoDB |
| Angular | Python | Redis |
| Vue | .NET / C# | MariaDB |
| Svelte | NodeJS | MySQL |
| | Deno | |
| | Rust | |
| | C | |
| | C++ | |
| | Ruby/Rails | |
| | Scala | |
| | Clojure | |
| | Kotlin | |
| | Swift | |
| | Dart | |
| | OCaml | |
| | Lisp | |
| | Haskell | |

Despite the long list of technologies in use, the only dependency needed to run the project is Docker. Docker is used as both the build, and runtime environment of each module. 

An orchestrator service (written in Go) running on the host machine uses the Docker Engine SDK to manage the modules. The orchestrator service hosts a web UI that allows the user to select which modules they'd like to run, and provides observability of the running modules. Module containers are built on-demand, and downed when swapping to a different module.

## Getting Started
Make sure Docker is installed on your machine.

- [Docker Desktop For Windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows)
- [Docker Desktop For Mac](https://hub.docker.com/editions/community/docker-ce-desktop-mac)
- [Docker Engine (Linux)](https://hub.docker.com/search?offering=community&q=&type=edition&operating_system=linux)

Clone this repository and run the orchestrator service -- there are pre-built 64-bit executable binaries for Windows, MacOS, and Linux in `/bin`. To run on other platforms and architectures see Building The Project (below). 

Once the orchestrator service is running, visit `localhost:4000` in a web browser to get started.

## Building The Project
There are pre-built 64-bit executable binaries for Windows, MacOS, and Linux in `/bin`.

If you need to build for other platforms, run the following command, setting `goos` and `goarch` to your OS and architecture of choice.

```
docker build \
    --output ./bin \
    --build-arg goos=<your-os-here> \
    --build-arg goarch=<your-arch-here> \
    -f Dockerfile.build .
```

Docker usually runs on 64-bit x86 architectures, if you're using something else I'll assume you know what you're doing. Here are some examples of supported `goos/goarch` values. Choose the values that match your machine, and plug them into the above command, replacing `<your-os-here>` and `<your-arch-here>` with the appropriate values.

`aix/ppc64`, `android/386`, `android/amd64`, `android/arm`, `android/arm64`, `darwin/amd64`, `darwin/arm64`, `dragonfly/amd64`, `freebsd/386`, `freebsd/amd64`, `freebsd/arm`, `freebsd/arm64`, `illumos/amd64`, `js/wasm`, `linux/386`, `linux/amd64`, `linux/arm`, `linux/arm64`, `linux/mips`, `linux/mips64`, `linux/mips64le`, `linux/mipsle`, `linux/ppc64`, `linux/ppc64le`, `linux/riscv64`, `linux/s390x`, `netbsd/386`, `netbsd/amd64`, `netbsd/arm`, `netbsd/arm64`, `openbsd/386`, `openbsd/amd64`, `openbsd/arm`, `openbsd/arm64`, `plan9/386`, `plan9/amd64`, `plan9/arm`, `solaris/amd64`, `windows/386`, `windows/amd64`, `windows/arm`

Example for 64-bit Windows:

```
docker build \
    --output ./bin \
    --build-arg goos=windows \
    --build-arg goarch=amd64 \
    -f Dockerfile.build .
```

An executable binary for the platform/architecture you selected will be built in a temporary docker container, and output to the `/bin` folder.