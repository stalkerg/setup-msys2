# Setup MSYS2

<p align="center">
  <a title="Dependency Status" href="https://david-dm.org/stalkerg/setup-msys2"><img src="https://img.shields.io/david/stalkerg/setup-msys2.svg?longCache=true&logo=npm&label=deps"></a><!--
  -->
  <a title="'action' workflow Status" href="https://github.com/stalkerg/setup-msys2/actions"><img alt="'action' workflow Status" src="https://github.com/stalkerg/setup-msys2/workflows/action/badge.svg"></a>
</p>

**setup-msys2** is a JavaScript GitHub Action (GHA) to setup a full-featured [MSYS2](https://www.msys2.org/) environment, using the GHA [toolkit](https://github.com/actions/toolkit).

The latest tarball available at [repo.msys2.org/distrib/x86_64](http://repo.msys2.org/distrib/x86_64/) is cached. Using the action extracts it and provides two entrypoints named `msys2` and `msys2do`.

## Usage

```yaml
  - uses: stalkerg/setup-msys2@v2
  - shell: msys2 {0}
    run: uname -a
```

```yaml
  - uses: stalkerg/setup-msys2@v2
  - run: msys2do uname -a
```

### Options

#### msystem

By default, `MSYSTEM` is set to `MINGW64`. However, an optional parameter named `msystem` is supported, which expects `MSYS`, `MINGW64` or `MING32`. For example:

```yaml
  - uses: stalkerg/setup-msys2@v2
    with:
      msystem: MSYS
```

Furthermore, the environment variable can be overriden. This is useful when multiple commands need to be executed in different contexts. For example, in order to build a PKGBUILD file and then test the installed artifact:

```yaml
  - uses: stalkerg/setup-msys2@v2
    with:
      msystem: MSYS
  - shell: msys2
    run: |
      makepkg-mingw -sCLfc --noconfirm --noprogressbar
      pacman --noconfirm -U mingw-w64-*-any.pkg.tar.xz
  - run: |
      set MSYSTEM=MINGW64
      msys2do <command to test the package>
```

#### path-type

By default, `MSYS2_PATH_TYPE` is set to `strict` by `msys2do`. It is possible to override it either using an option or setting the environment variable explicitly:

```yaml
  - uses: stalkerg/setup-msys2@v2
    with:
      path-type: inherit
  - run: msys2do <command>
```

```yaml
  - uses: stalkerg/setup-msys2@v2
  - run: msys2do <command>
    env:
      MSYS2_PATH_TYPE: inherit
```

#### update

By default, the installation is not updated; hence package versions are those of the installation tarball. By setting option `update` to `true`, the action will execute `pacman -Syu --no-confirm`:

```yaml
  - uses: stalkerg/setup-msys2@v2
    with:
      update: true
```
