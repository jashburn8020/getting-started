# Getting Started with Haskell

## GHCup

* GHCup is a universal installer for Haskell that will install everything you need to program in Haskell, and will help you manage those installations (update, switch versions, ...).
* Follow the instructions on the [GHCup webpage](https://www.haskell.org/ghcup/#) to install GHCup.
* Then, use it to install the Haskell toolchain, which consists of:
  * GHC (The Haskell compiler): You will mostly use a tool like Cabal or Stack to build your code, instead of GHC directly.
  * HLS (The Haskell Language Server): Your code editor will use it in the background.
  * Cabal (A Haskell build tool): You will use Cabal to structure your Haskell projects, build them, run them, define dependencies, etc.
  * Stack (A Haskell build tool) An alternative to Cabal.
* The toolchain requires some pre-requisite (Linux) packages.
  * To check if a package has already been installed: `apt list --installed | grep <package_name>`
* `ghcup` and the toolchain are installed into `~/.ghcup/`.

## References

* _Get Started._ (2025). Haskell.org. <https://www.haskell.org/get-started/>
