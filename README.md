# pam_python AUR Build Fix (Arch Linux)

Fixes build failures in the `pam_python` AUR package on modern Arch Linux systems by addressing missing GNU feature macros and overly strict compiler flags.

---

## Problem

While attempting to install **Howdy (Linux facial recognition)** on Arch Linux, the installation failed due to a broken dependency: `pam_python`.

The package fails to compile with errors such as:
pam_python.c: error: implicit declaration of function ‘strdup’
pam_python.c: error: implicit declaration of function ‘vsyslog’
error: command 'gcc' failed with exit status 1

---

## Root Cause

- Missing GNU feature macros prevented proper declaration of functions like `strdup` and `vsyslog`
- Legacy code assumptions incompatible with modern toolchains
- Use of `-Werror` caused warnings to be treated as fatal errors

---

## Fix

To resolve this, the PKGBUILD was modified to:

- Enable GNU extensions by adding -D_GNU_SOURCE to compilation flags
- Relax strict compiler behavior by removing -Werror and adding -Wno-error
- Adjust build configuration for compatibility with modern systems

With these changes, the package builds successfully and allows Howdy to be installed without issues.

To apply the fix, replace the original PKGBUILD in the pam-python AUR package with the modified version provided in this repository, then build using:
`makepkg -si`

## Changes (Diff)

See `changes.diff` for a full list of modifications.
