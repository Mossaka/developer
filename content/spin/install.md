title = "Install Spin"
template = "spin_main"
date = "2022-03-14T00:22:56Z"
enable_shortcodes = true
[extra]
url = "https://github.com/fermyon/developer/blob/main/content/spin/install.md"
keywords = "install"

---
- [Installing Spin](#installing-spin)
- [Verifying the Release Signature](#verifying-the-release-signature)
- [Building Spin From Source](#building-spin-from-source)
- [Using Cargo to Install Spin](#using-cargo-to-install-spin)
- [Installing Templates and Plugins](#installing-templates-and-plugins)
- [Next Steps](#next-steps)

## Installing Spin

Spin runs on Linux (amd64 and arm64), macOS (Intel and Apple Silicon), and Windows (amd64):

{{ tabs "os" }}

{{ startTab "Linux"}}

There are multiple ways to install Spin. The easiest is to use the installer script, hosted on this site. The script installs Spin along with a starter set of language templates and plugins:

<!-- @selectiveCpy -->

<pre class="bash spin-install" id="spin-install-linux"><code>$ curl -fsSL https://developer.fermyon.com/downloads/install.sh | bash</code></pre>

It's highly recommended to add Spin to a folder, which is on your path, e.g.:

<!-- @selectiveCpy -->

```bash
$ sudo mv spin /usr/local/bin/
```

> If you have already installed Spin by building from source, and then install it via the installer, we recommend you remove the older source install by running `cargo uninstall spin-cli`  Otherwise the Cargo path may take precedence over the "install from binary" path, and commands may get the "wrong" version of Spin. Use `spin --version` to confirm the version on the PATH is the one you intend, or `which spin` to confirm the path it is found on.

To install a specific version, you can pass arguments to the install script this way:

<!-- @selectiveCpy -->

<pre class="bash spin-install" id="spin-install-linux-version"><code>$ curl -fsSL https://developer.fermyon.com/downloads/install.sh | bash -s -- -v v0.10.0</code></pre>

To install the canary version of spin, you should pass the argument `-v canary`. The canary version is always the latest commit to the main branch of Spin:

<!-- @selectiveCpy -->

<pre class="bash spin-install" id="spin-install-linux-canary"><code>$ curl -fsSL https://developer.fermyon.com/downloads/install.sh | bash -s -- -v canary</code></pre>

{{ blockEnd }}

{{ startTab "macOS"}}

There are multiple ways to install Spin. The easiest is to use the installer script, hosted on this site. The script installs Spin along with a starter set of language templates and plugins:

<!-- @selectiveCpy -->

<pre class="bash spin-install" id="spin-install-macos"><code>$ curl -fsSL https://developer.fermyon.com/downloads/install.sh | bash</code></pre>

It's highly recommended to add Spin to a folder, which is on your path, e.g.:

<!-- @selectiveCpy -->

```bash
$ sudo mv spin /usr/local/bin/
```

> If you have already installed Spin by building from source, and then install it via the installer, we recommend you remove the older source install by running `cargo uninstall spin-cli`  Otherwise the Cargo path may take precedence over the "install from binary" path, and commands may get the "wrong" version of Spin. Use `spin --version` to confirm the version on the PATH is the one you intend, or `which spin` to confirm the path it is found on.

To install a specific version, you can pass arguments to the install script this way:

<!-- @selectiveCpy -->

<pre class="bash spin-install" id="spin-install-macos-version"><code>$ curl -fsSL https://developer.fermyon.com/downloads/install.sh | bash -s -- -v v0.10.0</code></pre>

To install the canary version of spin, you should pass the argument `-v canary`. The canary version is always the latest commit to the main branch of Spin:

<!-- @selectiveCpy -->

<pre class="bash spin-install" id="spin-install-macos-canary"><code>$ curl -fsSL https://developer.fermyon.com/downloads/install.sh | bash -s -- -v canary</code></pre>

{{ blockEnd }}

{{ startTab "Windows"}}

If using Windows (PowerShell / cmd.exe), you can download <a href="https://github.com/fermyon/spin/releases/latest" class="spin-install" id="spin-install-windows">the Windows binary release of Spin</a>.

Simply unzip the binary release and place the `spin.exe` in your system path.

This does not install any Spin templates or plugins. For a starter list, see the [Installing Templates and Plugins section](#installing-templates-and-plugins).

If you want to use WSL2 (Windows Subsystem for Linux 2), please follow the instructions for using Linux.

{{ blockEnd }}
{{ blockEnd }}

## Verifying the Release Signature

The Spin project [signs releases](https://github.com/fermyon/spin/blob/main/docs/content/sips/012-signing-spin-releases.md) using [Sigstore](https://docs.sigstore.dev/), a project that helps with signing software and _stores signatures in a tamper-resistant public log_. Consumers of Spin releases can validate the integrity of the package they downloaded by performing a validation of the artifact against the signature present in the public log. Specifically, users get two main guarantees by verifying the signature: 1) that the author of the artifact is indeed the one expected (i.e. the build infrastructure associated with the Spin project, at a given revision that can be inspected), and 2) that the content generated by the build infrastructure has not been tampered with.

To verify the release signature, first [configure Cosign v2.0.0+](https://docs.sigstore.dev/cosign/installation/). This is the CLI tool that we will use validate the signature.
The same directory where the installation script was run should also contain a signature of the Spin binary and the certificate used to perform the signature. The following command will perform the signature verification using the `cosign` CLI:

<!-- @selectiveCpy -->

<pre class="bash spin-install" id="spin-install-source-build"><code> $ cosign verify-blob \
    --signature spin.sig \
    --certificate crt.pem \
    --certificate-identity https://github.com/fermyon/spin/.github/workflows/release.yml@refs/tags/<version> \
    --certificate-oidc-issuer https://token.actions.githubusercontent.com \
    # --certificate-github-workflow-sha <optionally, pass the commit SHA associated with the tag> \
    ./spin
Verified OK
</code></pre>

You can now move the Spin binary to the path knowing that it was indeed built by the infrastructure associated with the Spin project, and that it has not been tampered with since the build.

## Building Spin From Source

[Follow the contribution document](./contributing-spin.md) for a detailed guide on building Spin from source:

<!-- @selectiveCpy -->

<pre class="bash spin-install" id="spin-install-source-build"><code>$ git clone https://github.com/fermyon/spin
$ cd spin && make build
$ ./target/release/spin --help
</code></pre>

> Please note: On a fresh Linux installation, you will also need the standard build toolchain (`gcc`, `make`, etc.), the SSL library headers, and on some distributions you may need `pkg-config`. For example, on Debian-like distributions, including Ubuntu, you can install the standard build toolchain with this command:

<!-- @selectiveCpy -->

```bash
$ sudo apt-get install build-essential libssl-dev pkg-config
```

This does not install any Spin templates or plugins. For a starter list, see the [Installing Templates and Plugins section](#installing-templates-and-plugins).

## Using Cargo to Install Spin

If you have [`cargo`](https://doc.rust-lang.org/cargo/getting-started/installation.html), you can clone the repo and install it to your path:

<!-- @selectiveCpy -->

<pre class="bash spin-install" id="spin-install-source-cargo"><code>$ git clone https://github.com/fermyon/spin -b v1.2.1
$ cd spin
$ rustup target add wasm32-wasi
$ rustup target add wasm32-unknown-unknown
$ cargo install --locked --path .
$ spin --help
</code></pre>

> Please note: Installing Spin v1.2.1 from source requires Rust 1.68.0 or newer. You can update Rust using the following command:

<!-- @selectiveCpy -->

```bash
$ rustup update
```

This does not install any Spin templates or plugins. For a starter list, see the [Installing Templates and Plugins section](#installing-templates-and-plugins).

## Installing Templates and Plugins

Spin has a variety of templates and plugins to make it easier to create Spin applications in your favorite programming language. 
The install script automatically installs a starter set of templates and plugins, namely templates from the Spin repository and JavaScript and Python toolchain plugins and the Fermyon Cloud plugin. If you used a different installation method, we recommend you install these templates and plugins manually, as follows:

```sh
spin templates install --git https://github.com/fermyon/spin --upgrade
spin templates install --git https://github.com/fermyon/spin-python-sdk --upgrade
spin templates install --git https://github.com/fermyon/spin-js-sdk --upgrade
spin plugins update
spin plugins install js2wasm --yes
spin plugins install py2wasm --yes
spin plugins install cloud --yes
```

To list installed templates, run:

```sh
spin templates list
```

To list installed and available plugins, run:

```sh
spin plugins list
```

## Next Steps

- [Take Spin for a spin](./quickstart.md)
- Learn about how to [write a Spin application](writing-apps)
- Try the [Fermyon Cloud](/cloud/quickstart)
