Portage Binpkg Support Overlay
==============================

This overlay contains a version of sys-apps/portage-9999 with enhanced binary package support, which pulls from the
[binpkg-support-integration branch](https://github.com/zmedico/portage/tree/binpkg-support-integration).

Usage
-----

Add this repository to /etc/portage/repos.conf, using a configuration like this:
```
[portage-binpkg-support]
location = /var/portage/repos/portage-binpkg-support
sync-type = git
sync-uri = https://github.com/zmedico/portage-binpkg-support-overlay.git
auto-sync = true
```

Then run `emerge --sync`, followed by `emerge =sys-apps/portage-9999::portage-binpkg-support`.

In order to enable support for multiple binary package instances in PKGDIR per ebuild, set
FEATURES="binpkg-multi-instance" in /etc/portage/make.conf or in make.defaults of your profile.
In order to allow a dependency atom in your profiles to select a specific build, set
"profile-formats = build-id" in metadata/layout.conf of the repository that contains your
profiles. An atom like `=sys-apps/portage-2.2.17-1` will select the binary package instance
for sys-apps/portage-2.2.17 with build-id 1. The binary package can be fetched from a binhost
via emerge --getbinpkg, or stored locally as `${PKGDIR}/sys-apps/portage/portage-2.2.17-1.xpak`.

In order to enable soname dependency resolution, use emerge --ignore-soname-deps=n, and refer
to the emerge(1) man page for more information about this option. Note that --usepkgonly or
--getbinpkgonly must be enabled in order for soname dependency resolution to work for
installation actions. I've put my
[PROVIDES_EXCLUDE and REQUIRES_EXCLUDE package.bashrc](https://github.com/zmedico/portage-soname-bashrc)
settings in a git repo, to provide some examples of how PROVIDES_EXCLUDE and REQUIRES_EXCLUDE
are useful in practice. With these settings in /etc/portage/profile/package.bashrc, all soname
dependencies on my desktop multilib system are resolvable (2283 packages, including KDE 4.14.3).


Included features not upstream
------------------------------

* [FEATURES=binpkg-multi-instance](https://github.com/zmedico/portage/tree/multi-binpkg-per-ebuild) (support is nearing completion)

* [Soname dependencies](https://github.com/zmedico/portage/tree/binpkg-soname-deps) (support is nearing completion)
