# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

"Cobalt Forge": a personal learning lab for building a local X.509 PKI with OpenSSL 3, ending with a local Spring Boot test site (the first of several planned stacks) served over HTTPS with a certificate the machine trusts. The main "work" is writing step-by-step lab guides (in **Spanish**) and running the OpenSSL commands they describe. There is no test suite.

Target platforms are **macOS, Windows and Linux**. Today macOS is fully covered, Windows partially (labs 00, 05–07) and Linux not yet; the coverage table in `README.md` tracks this — update it when adding platform sections. New or edited labs should include per-OS subsections where commands differ (keep OpenSSL commands shared when they are identical).

The repo lives at `~/Developer/Learning/Cobalt-Forge-Lab/pki-lab/` (formerly `~/Developer/Learning/open-ssl/`, then `Cobalt-Forge-Lab/cobalt-forge-pki-lab/`). The parent `Cobalt-Forge-Lab/` also holds `notes/` and `ideas/` (not versioned here) and will host future sibling labs (SSH, PGP) as separate directories. Keep this repo self-contained: don't reference sibling paths from lab commands.

## Layout

- `docs/NN-<topic>...md` — the numbered lab guides, each continuing the previous one. Labs: `00` install Java 21 + run the site over HTTP; `01` Root CA; `02` Intermediate CA; `03` server cert for `lab.cobalt-forge.test` (unencrypted RSA 2048 key, `cert/server.ext` with SAN/`serverAuth`, signed by the intermediate, 397 days); `04` PKCS#12 keystore `cert/lab.cobalt-forge.test.p12` (alias `cobalt-forge`, includes the intermediate); `05` `hosts` entry; `06` HTTPS via a Spring profile `https` on port 8443, ending in the untrusted-CA browser warning; `07` trust the Root CA in the OS store (macOS login keychain / Windows `certutil -user`) and see the padlock. Each doc ends with a link to the next.
- `cert/` — output directory for **every** OpenSSL command in the labs (keys, CSRs, certs, `.ext`, `rootCA.srl`), flat. All lab commands run from the project root and write to `cert/...`; new labs must keep this (first step: `mkdir -p cert`). Everything in `cert/` except `cert/README.md` is git-ignored, and `.gitignore` also blocks `*.pem`, `*.key`, `*.csr`, etc. repo-wide. Never stage key material.
- `web-java/` — the "Cobalt Forge" test site: a minimal Spring Boot 4.1 app (Java 21, `spring-boot-starter-webmvc` + Thymeleaf, package `test.cobaltforge.lab`) with Maven Wrapper. `HomeController` serves `GET /` → `templates/index.html`, showing whether the request is HTTPS (`request.isSecure()`), host and Java version. Static assets in `src/main/resources/static/`: vendored Pico CSS v2.1.1 (`css/pico.min.css`, don't edit) and `css/theme.css`, which holds the cobalt palette (`--cobalt-50…950`, `--cobalt-700` = `#0047AB`, `--ember-400` accent), the `@font-face` rules for `fonts/`, and the mapping of Pico's `--pico-*` variables under `[data-theme="dark"]`. Change colors only in `theme.css`. `application.yml` is HTTP-only on port 8080. HTTPS lives in `application-https.yml` (port 8443, `server.ssl.key-store: file:../cert/lab.cobalt-forge.test.p12`, password from env `COBALT_FORGE_P12_PASSWORD`), which the user creates while following lab 06 — it is not committed ahead of time. Run with `./mvnw spring-boot:run -Dspring-boot.run.profiles=https` from `web-java/`.

## The PKI being built

Root CA (`CN=Lab Root CA`, self-signed, RSA 4096, 3650 days, `pathlen:1`) → Intermediate CA (`CN=Lab Intermediate CA`, 1825 days, `pathlen:0`) → future end-entity server/client certs. Subject fields use `/C=CO/ST=Bogota/L=Bogota/O=PKI Lab/CN=...`. Keys are RSA 4096 encrypted with AES-256-CBC (password-protected), signatures SHA-256. New labs should stay consistent with these names, parameters, and the `<name>CA.{key,csr,cert}.pem` file naming.

## Commands

```bash
cd web-java
./mvnw spring-boot:run          # http://localhost:8080
./mvnw -DskipTests package      # builds target/cobalt-forge-lab-0.1.0.jar
```

Java 21 comes from SDKMAN; the default `java` on this machine may be older, so select it first (`sdk use java 21...`) or set `JAVA_HOME` to `~/.sdkman/candidates/java/21.*`.

## Running OpenSSL

macOS's `/usr/bin/openssl` is LibreSSL; the labs always invoke Homebrew's OpenSSL 3 via a variable:

```bash
export OPENSSL="$(brew --prefix openssl@3)/bin/openssl"
```

Signing and decrypting keys prompt for passwords interactively, so commands that use private keys must be run by the user (suggest `! <command>`). Read-only inspection of certs is safe to run directly, e.g.:

```bash
$OPENSSL x509 -in cert/intermediateCA.cert.pem -noout -subject -issuer -dates
$OPENSSL verify -CAfile cert/rootCA.cert.pem -show_chain cert/intermediateCA.cert.pem
```

Do not regenerate or overwrite `rootCA.key.pem` / `rootCA.cert.pem` — everything downstream is signed by them.

## Doc conventions

Follow the style of `docs/01` and `docs/02`: Spanish prose; a "solo para aprendizaje" disclaimer; commands written with `$OPENSSL` and one option per line with `\` continuations; tables explaining each X.509 extension; expected output shown in `text` blocks; a `chmod` step (400 for keys, 444 for public files); Mermaid flowcharts (hierarchy with `existing`/`current`/`future` classDefs, process flow, key/cert relationships); and links to official OpenSSL 3 man pages.
