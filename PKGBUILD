_major=5
_minor=3
#_patchlevel=0
#_subversion=1
_basekernel=${_major}.${_minor}
_srcname=linux-${_basekernel}
pkgbase=linux-pf
_pfrel=3
_kernelname=-pf
_pfpatchhome="https://github.com/pfactum/pf-kernel/compare"
_pfpatchname="v$_major.$_minor...v$_major.$_minor-pf$_pfrel.diff"
_CPUSUFFIXES_KBUILD=(
  CORE2 K7 K8 K10 BARCELONA BOBCAT BULLDOZER PILEDRIVER PSC
  ATOM PENTIUMII PENTIUMIII PENTIUMM PENTIUM4 NEHALEM SANDYBRIDGE
  IVYBRIDGE HASWELL BROADWELL SILVERMONT SKYLAKE)
pkgname=('linux-pf')
pkgdesc="Linux kernel and modules with the pf-kernel patch (uksm, PDS)."
pkgname=('linux-pf' 'linux-pf-headers' 'linux-pf-preset-default')
pkgver=${_basekernel}.${_pfrel}
pkgrel=3
arch=('i686' 'x86_64')
url="https://gitlab.com/post-factum/pf-kernel/wikis/README"
license=('GPL2')
options=('!strip')
makedepends=('git' 'xmlto' 'docbook-xsl' 'xz' 'bc' 'kmod' 'elfutils' 'cpio')
source=("https://www.kernel.org/pub/linux/kernel/v${_major}.x/linux-${_basekernel}.tar.xz"
	      'config.ACE'
	      "${_pfpatchhome}/${_pfpatchname}"	# the -pf patchset
        "90-linux.hook"
        "60-linux.hook"
       )
# 	'cx23885_move_CI_AC_registration_to_a_separate_function.patch'     



prepare() {
  cd "${srcdir}/linux-${_basekernel}"
  msg "Applying pf-kernel patch"
  patch -Np1 < ${srcdir}/${_pfpatchname}

  # linux-ARCH patches

  # add latest fixes from stable queue, if needed
  # http://git.kernel.org/?p=linux/kernel/git/stable/stable-queue.git
  
  # fixed by -pf now
  # disable USER_NS for non-root users by default
  #patch -Np1 -i ../0001-add-sysctl-to-disallow-unprivileged-CLONE_NEWUSER-by.patch
  
  # https://bugs.archlinux.org/task/56711
  #patch -Np1 -i ../0002-drm-i915-edp-Only-use-the-alternate-fixed-mode-if-it.patch
  # end

  # end linux-ARCH patches
  
  # fix ci  invalid PC card inserted issue hopefully
  #patch -Rp1 -i "${srcdir}/cx23885_move_CI_AC_registration_to_a_separate_function.patch" || true

  
	  cat "${startdir}/config.ACE" > .config

  sed -ri "s|SUBLEVEL = 0|SUBLEVEL = $_pfrel|" Makefile

  # Set EXTRAVERSION to -pf
  #sed -ri "s|^(EXTRAVERSION =).*|\1 |" Makefile
  # _arch=$CARCH

  
  # If the following is set, stop right there. We only need the headers for
  # dependent drivers' compiling (nvidia, virtualbox etc)

  # merge our changes to arches kernel config
  #./scripts/kconfig/merge_config.sh .config "$srcdir"/pf_defconfig

  # get kernel version
  #make prepare
}

build() {
  cd "${srcdir}/linux-${_basekernel}"
  make ${MAKEFLAGS} LOCALVERSION= bzImage modules
}

_package() {
  _pkgdesc="Linux kernel and modules with the pf-kernel patch (uksm, PDS)."
  pkgdesc=${_pkgdesc}
  groups=('base')
  depends=('coreutils' 'linux-firmware' 'kmod>=9-2' 'mkinitcpio>=0.7' 'linux-pf-preset')
  optdepends=('linux-docs: Kernel hackers manual - HTML documentation that comes with the Linux kernel.'
	            'crda: to set the correct wireless channels of your country'
	            'pm-utils: utilities and scripts for suspend and hibernate power management'
	            'nvidia-pf: NVIDIA drivers for linux-pf'
	            'nvidia-beta-all: NVIDIA drivers for all installed kernels'
              'uksmd: Userspace KSM helper daemon'
	            'modprobed-db: Keeps track of EVERY kernel module that has ever been probed. Useful for make localmodconfig.')
  provides=('linux-tomoyo')
  replaces=('kernel26-pf')

  cd "${srcdir}/linux-${_basekernel}"

  if [[ "$_PKGOPT" = "y" ]]; then	# package naming according to optimization
    case $CPU in
      CORE2)
        pkgname="${pkgbase}-core2"
        pkgdesc="${_pkgdesc} Intel Core2 optimized."
        ;;
      K7)
        pkgname="${pkgbase}-k7"
        pkgdesc="${_pkgdesc} AMD K7 optimized."
        ;;
      K8)
        pkgname="${pkgbase}-k8" 
        pkgdesc="${_pkgdesc} AMD K8 optimized."
	      ;;
      K10)
        pkgname="${pkgbase}-k10"
	      pkgdesc="§{_pkgdesc} AMD K10 optimized"
        ;;
      BARCELONA)
        pkgname="${pkgbase}-barcelona"
        pkgdesc="${_pkgdesc} AMD Barcelona optimized."
	      ;;
      BOBCAT)
	      pkgname="${pkgbase}-bobcat"
	      pkgdesc="${_pkgdesc} AMD Bobcat optimized."
	      ;;
      BULLDOZER)
	      pkgname="${pkgbase}-bulldozer"
	      pkgdesc="${_pkgdesc} AMD Bulldozer optimized."
	      ;;
      PILEDRIVER)
	      pkgname="${pkgbase}-piledriver"
	      pkgdesc="${_pkgdesc} AMD Piledriver optimized."
	      ;;
      PSC)
        pkgname="${pkgbase}-psc"
        pkgdesc="${_pkgdesc} Intel Pentium4/D/Xeon optimized."
        ;;
      ATOM)
        pkgname="${pkgbase}-atom"
        pkgdesc="${_pkgdesc} Intel Atom optimized."
        ;;
      PENTIUMII)
        pkgname="${pkgbase}-p2"
        pkgdesc="${_pkgdesc} Intel Pentium2 optimized."
        ;;
      PENTIUMIII)
        pkgname="${pkgbase}-p3"
        pkgdesc="${_pkgdesc} Intel Pentium3 optimized."
        ;;
      PENTIUMM)
        pkgname="${pkgbase}-pm"
        pkgdesc="${_pkgdesc} Intel Pentium-M optimized."
        ;;
      PENTIUM4)
        pkgname="${pkgbase}-p4"
        pkgdesc="${_pkgdesc} Intel Pentium4 optimized."
        ;;
      NEHALEM)
	      pkgname="${pkgbase}-nehalem"
        pkgdesc="${_pkgdesc} Intel Core Nehalem optimized."
	      ;;
      SANDYBRIDGE)
        pkgname="${pkgbase}-sandybridge"
        pkgdesc="${_pkgdesc} Intel 2nd Gen Core processors including Sandy Bridge."
	      ;;
      IVYBRIDGE)
        pkgname="${pkgbase}-ivybridge"
        pkgdesc="${_pkgdesc} Intel 3rd Gen Core processors including Ivy Bridge."
	      ;;
      HASWELL)
        pkgname="${pkgbase}-haswell"
        pkgdesc="${_pkgdesc} 4th Gen Core processors including Haswell."
	      ;;
      BROADWELL)
        pkgname="${pkgbase}-broadwell"
        pkgdesc="${_pkgdesc} 5th Gen Core processors including Broadwell."
	      ;;
      SILVERMONT)
        pkgname="${pkgbase}-silvermont"
        pkgdesc="${_pkgdesc} 6th Gen Core processors including Silvermont."
	      ;;
      SKYLAKE)
        pkgname="${pkgbase}-skylake"
        pkgdesc="${_pkgdesc} 6th Gen Core processors including Skylake."
        ;;
    esac


    if [[ "$pkgname" != "$pkgbase" ]]; then
      # If optimized build, conflict with generi
      conflicts=('linux-pf')
      provides+=(${pkgbase}=$pkgver) 
    fi
  fi

  echo
  echo "    ========================================"
  msg  "The packages will be named ${pkgname}"
  msg  "${pkgdesc}"
  echo "    ========================================"
  echo

  ### package_linux-pf

  ### c/p from linux-ARCH

  cd "${srcdir}/linux-${_basekernel}"

  KARCH=x86

  echo # get kernel version
  _kernver="$(make LOCALVERSION= kernelrelease)"

  mkdir -p "${pkgdir}"/usr/lib/modules
  make LOCALVERSION= INSTALL_MOD_PATH="${pkgdir}/usr" modules_install


  msg2 "Installing boot image..."
  local image="$pkgdir/boot/vmlinuz-$pkgbase"
  install -Dm644 "$(make -s image_name)" "$image"

  # systemd expects to find the kernel here to allow hibernation
  # https://github.com/systemd/systemd/commit/edda44605f06a41fb86b7ab8128dcf99161d2344
  ln -sr "$image" "${pkgdir}/usr/lib/modules/vmlinuz"

  
  # make room for external modules
  local _extramodules="extramodules-${_basekernel}${_kernelname:--ARCH}"
  ln -s "../${_extramodules}" \
     "${pkgdir}/usr/lib/modules/${_kernver}/extramodules"

  # add real version for building modules and running depmod from hook
  echo "${_kernver}" |
    install -Dm644 /dev/stdin \
            "${pkgdir}/usr/lib/modules/${_extramodules}/version"

  
  # remove build and source links
  rm "${pkgdir}"/usr/lib/modules/${_kernver}/{source,build}
  
  # add vmlinux
  install -Dt "${pkgdir}/usr/lib/modules/${_kernver}/build" -m644 vmlinux

  msg2 "Fixing permissions..."
  chmod -Rc u=rwX,go=rX "$pkgdir"
  
  # end c/p
}

### package_linux-pf-headers
_package-headers() {
  pkgname=${pkgbase}-headers
  pkgdesc="Header files and scripts for building modules for linux-pf kernel."

  local _builddir="${pkgdir}/usr/lib/modules/${_kernver}/build"
  
  cd "${srcdir}/${_srcname}"

  # only install objtool when stack validation is enabled
  if grep -q CONFIG_STACK_VALIDATION=y .config   ; then
    install -Dt "${_builddir}/tools/objtool" tools/objtool/objtool
  fi


  msg2 "Installing build files..."
  install -dm755 "${_builddir}" 
  install -Dt "${_builddir}" -m644 Makefile .config Module.symvers
  install -Dt "${_builddir}/kernel" -m644 kernel/Makefile

  
  install -D -m644 arch/${KARCH}/Makefile -t "${_builddir}/arch/${KARCH}/"
  
  if [ "${CARCH}" = "i686" ]; then
    install -Dm644 arch/${KARCH}/Makefile_32.cpu -t "${_builddir}/arch/${KARCH}/"
  fi

  # copy files necessary for later builds, like nvidia and vmware
  cp -a scripts "${_builddir}"

 
  msg2 "Installing headers..."
  cp -t "$_builddir" -a include
  # copy arch includes for external modules
  cp -t "$_builddir/arch/x86" -a arch/x86/include


  # fix permissions on scripts dir
  chmod og-w -R "${_builddir}/scripts"
  mkdir -p "${_builddir}/.tmp_versions"


  install -D -m644 -t "${_builddir}/arch/${KARCH}/kernel/" arch/${KARCH}/kernel/asm-offsets.s

  install -Dt "${_builddir}/drivers/md" -m644 drivers/md/*.h
  install -Dt "${_builddir}/net/mac80211" -m644 net/mac80211/*.h
  
  # http://bugs.archlinux.org/task/13146
  install -Dt "${_builddir}/drivers/media/i2c" -m644 drivers/media/i2c/msp3400-driver.h
  
  # http://bugs.archlinux.org/task/20402
  install -Dt "${_builddir}/drivers/media/usb/dvb-usb" -m644 drivers/media/usb/dvb-usb/*.h
  install -Dt "${_builddir}/drivers/media/dvb-frontends" -m644 drivers/media/dvb-frontends/*.h
  install -Dt "${_builddir}/drivers/media/tuners" -m644 drivers/media/tuners/*.h
  
  # and...
  # http://bugs.archlinux.org/task/11194
  ###
  ### DO NOT MERGE OUT THIS IF STATEMENT
  ### IT AFFECTS USERS WHO STRIP OUT THE DVB STUFF SO THE OFFICIAL ARCH CODE HAS A CP
  ### LINE THAT CAUSES MAKEPKG TO END IN AN ERROR
  ###
  if [ -d include/config/dvb/ ]; then
    install -Dm644 -t "${_builddir}/include/config/dvb/" include/config/dvb/*.h 
  fi
  
  # add xfs and shmem for aufs building
  mkdir -p "${_builddir}/"{fs/xfs,mm}

  msg2 "Installing KConfig files..."
  find . -name Kconfig\* -exec install -Dm644 {} "${_builddir}/{}" \;
    
  # strip scripts directory
  find "${_builddir}/scripts" -type f -perm -u+w 2>/dev/null | while read binary ; do
    case "$(file -bi "${binary}")" in
      *application/x-sharedlib*) # Libraries (.so)
        /usr/bin/strip ${STRIP_SHARED} "${binary}";;
      *application/x-archive*) # Libraries (.a)
        /usr/bin/strip ${STRIP_STATIC} "${binary}";;
      *application/x-executable*) # Binaries
        /usr/bin/strip ${STRIP_BINARIES} "${binary}";;
    esac
  done
  
  msg2 "Removing unneeded architectures..."
  local arch
  for arch in "$_builddir"/arch/*/; do
    [[ $arch = */x86/ ]] && continue
    echo "Removing $(basename "$arch")"
    rm -r "$arch"
  done

  # remove files already in linux-docs package
  msg2 "Removing documentation..."
  rm -r "$_builddir/Documentation"
  
  # Add version.h for dumb binary blob installers which aren't
  cd "${_builddir}/include/linux/"
  [[ -e version.h ]] || ln -s ../generated/uapi/linux/version.h


  msg2 "Adding symlink..."
  mkdir -p "$pkgdir/usr/src"
  ln -sr "$_builddir" "$pkgdir/usr/src/$pkgbase-$pkgver"

  # Fix permissions
  chmod -R u=rwX,go=rX "${_builddir}"
}
_package-preset-default()
{
  pkgname=linux-pf-preset-default
  provides=( "linux-pf-preset=$pkgver")
  pkgdesc="Linux-pf default preset"
  install=linux.install
  depends=("linux-pf=$pkgver")
  backup=("etc/mkinitcpio.d/${pkgbase}.preset")

  # install fallback mkinitcpio.conf file and preset file for kernel
  install -D -m644 "${startdir}/linux.preset" "${pkgdir}/etc/mkinitcpio.d/${pkgbase}.preset"

  # sed expression for following substitutions
  local _subst="
    s|%PKGBASE%|${pkgbase}|g
    s|%KERNVER%|${_kernver}|g
    s|%EXTRAMODULES%|${_extramodules}|g
  "

  # hack to allow specifying an initially nonexisting install file
  sed "${_subst}" "${startdir}/${install}" > "${startdir}/${install}.pkg"
  true && install=${install}.pkg

  # install mkinitcpio preset file
  #sed "${_subst}" ../linux-pf.preset |
  #  install -Dm644 /dev/stdin "${pkgdir}/etc/mkinitcpio.d/${pkgbase}.preset"
  
  # install pacman hooks
  sed "${_subst}" "${srcdir}"/60-linux.hook |
    install -Dm644 /dev/stdin "${pkgdir}/usr/share/libalpm/hooks/60-${pkgbase}.hook"
  sed "${_subst}" "${srcdir}"/90-linux.hook |
    install -Dm644 /dev/stdin "${pkgdir}/usr/share/libalpm/hooks/90-${pkgbase}.hook"
  
  # set correct depmod command for install
  #sed \
    #  -e  "s/KERNEL_NAME=.*/KERNEL_NAME=${_kernelname}/" \
    #  -e  "s/KERNEL_VERSION=.*/KERNEL_VERSION=${_kernver}/" \
    #  -i "${startdir}/linux.install"
  sed \
    -e "1s|'linux.*'|'${pkgbase}'|" \
    -e "s|ALL_kver=.*|ALL_kver=\"/boot/vmlinuz-${pkgbase}\"|" \
    -e "s|default_image=.*|default_image=\"/boot/initramfs-${pkgbase}.img\"|" \
    -e "s|fallback_image=.*|fallback_image=\"/boot/initramfs-${pkgbase}-fallback.img\"|" \
    -i "${pkgdir}/etc/mkinitcpio.d/${pkgbase}.preset"
}

for _p in linux-pf-headers linux-pf-preset-default ; do
  eval "package_${_p}() {
    $(declare -f "_package${_p#${pkgbase}}")
    _package${_p#${pkgbase}}
  }"
done
if [ "$makepkg_version" ] ; then
  if in_array ${source[*]} batch_opts ; then #FIXME bugs updpkgsums
    source batch_opts
  fi
fi

pkgname[0]=linux-pf${LCPU+-}${LCPU}

eval "package_linux-pf${LCPU+-$LCPU}() {
     $(declare -f "_package")
     _package
     }"


sha256sums=('78f3c397513cf4ff0f96aa7d09a921d003e08fa97c09e0bb71d88211b40567b2'
            'b3923386539ed3270f88b8988adf8d0cad5f43ab2d127569e90f46da5f793cbd'
            '87774dc640aae7f6b1e4f338cda0ce7eec35fa73f2897e697ae43e10629bc085'
            '75f99f5239e03238f88d1a834c50043ec32b1dc568f2cc291b07d04718483919'
            'ae2e95db94ef7176207c690224169594d49445e04249d2499e9d2fbc117a0b21')
# vim:set ts=2 sw=2 tw=0 et:
