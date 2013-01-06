# Maintainer: Christopher Reimer <c[dot]reimer[at]googlemail[dot]com>
pkgname=vdr
pkgver=1.7.35
pkgrel=1yavdr1
pkgdesc="'open' digital satellite receiver and timer controlled video disk recorder"
url="http://tvdr.de/"
arch=('x86_64' 'i686')
license=('GPL2')
depends=('libjpeg-turbo' 'perl' 'ttf-dejavu')
optdepends=('lirc: remote control support'
            'runvdr-extreme: startscript for vdr')
provides=("vdr-api=$pkgver")
backup=('var/lib/vdr/channels.conf'
        'var/lib/vdr/diseqc.conf'
        'var/lib/vdr/keymacros.conf'
        'var/lib/vdr/scr.conf'
        'var/lib/vdr/sources.conf'
        'var/lib/vdr/svdrphosts.conf')
options='!emptydirs'
install='vdr.install'
source=("ftp://ftp.tvdr.de/vdr/Developer/${pkgname}-${pkgver}.tar.bz2"
        'MainMenuHooks-v1_0_2.diff::http://www.vdr-portal.de/index.php?page=Attachment&attachmentID=30330'
        'shutdown.sh'
        'shutdown-wrapper.c'
        'osdbase-maxitems.diff'
        'valgrind.diff'
        'vasarajanauloja.diff'
        'x_edit_marks.diff'
        'jumpplay.diff'
        'ttxtsubs.diff'
        'x_menuorg.diff'
        'x_timer-info.diff'
        'rotor.diff'
        'graphtft.diff'
        'graphtft-liemikuutio.diff'
        'wareagleicon.diff'
        'eventdetails_v3.diff'
        'vdr-remote_instant_recordings.diff'
        'dynamite.diff'
        'pin.diff'
        'jumpingseconds.diff'
        'epgsearch-exttimeredit-0.0.2.diff'
        'xprmtl-01_externalci.diff')
md5sums=('3b9d0376325370afb464b6c5843591c7'
         '301c9b9766ed5182b07f1debc79abc21'
         'ef8e11062f58a9eb4016dfbf66bb9044'
         '7cad811b4ac5ee6c0b5496d006f1e0ee'
         '2d75fd8722f222633ea616febd43172a'
         'f7f6b9f4c2b1f569486cad17795eef0d'
         '7e5809fd70bf140c38b1d6b8a5b95b71'
         '66febac31c90fddc1ed4515537271e1c'
         '30b5d23cd93f7c656b583f8233313a0f'
         '8956490773e9409e7f429c0173092035'
         'b1c3fa683891c96e36b2357b3de2c244'
         '37dc38c833b7be7878670aefe3013c84'
         'f6b2652251e588268e51789837856606'
         'ddc60900a073ffd67098b40de8d48014'
         '5f32d384862e650feb69240f1343467d'
         'b536fa0134b2bc5aa3f68113c9cc2be3'
         '2b4456386ae9edff9f291965089e1dca'
         'ecf8fe3bce6a72df6821f819f71be2d6'
         'a3ab75672a14b7bc5b1ab06a19e65832'
         '97ac800864372b29693ecd9324c00079'
         'c190116317643514ef1224c15181e769'
         '0f58cc0a4cb84216febca8bb03855593'
         'e3018956f0820def3bb9d9ea1e586822')

build() {
  gcc -o shutdown-wrapper shutdown-wrapper.c

  cd "${srcdir}/${pkgname}-${pkgver}"

cat > Make.config <<EOF
PREFIX       = /usr
INCDIR       = /usr/include
LOCDIR       = /usr/share/locale
LIBDIR       = /usr/lib/vdr/plugins
VIDEODIR     = /srv/vdr/video
CONFDIR      = /var/lib/vdr
CACHEDIR     = /var/cache/vdr
RESDIR       = /usr/share/vdr
VDR_USER     = vdr
EOF

  patch -p1 -i ${srcdir}/MainMenuHooks-v1_0_2.diff
  patch -p1 -i ${srcdir}/osdbase-maxitems.diff
  patch -p1 -i ${srcdir}/valgrind.diff
  patch -p1 -i ${srcdir}/vasarajanauloja.diff
  patch -p1 -i ${srcdir}/x_edit_marks.diff
  patch -p1 -i ${srcdir}/jumpplay.diff
  patch -p1 -i ${srcdir}/ttxtsubs.diff
  patch -p1 -i ${srcdir}/x_menuorg.diff
  patch -p1 -i ${srcdir}/x_timer-info.diff
  patch -p1 -i ${srcdir}/rotor.diff
  patch -p1 -i ${srcdir}/graphtft.diff
  patch -p1 -i ${srcdir}/graphtft-liemikuutio.diff
  patch -p1 -i ${srcdir}/wareagleicon.diff
  patch -p1 -i ${srcdir}/eventdetails_v3.diff
  patch -p1 -i ${srcdir}/vdr-remote_instant_recordings.diff
  patch -p1 -i ${srcdir}/dynamite.diff
  patch -p1 -i ${srcdir}/pin.diff
  patch -p1 -i ${srcdir}/jumpingseconds.diff
  patch -p1 -i ${srcdir}/epgsearch-exttimeredit-0.0.2.diff
  patch -p1 -i ${srcdir}/xprmtl-01_externalci.diff

  # Build VDR with recommended optimization level
  CFLAGS+=' -O3'
  CXXFLAGS+=' -O3 -fPIC'


  # Workaround for missing include in plugin makefiles
  CFLAGS+=" -I$(pwd)/include"
  CXXFLAGS+=" -I$(pwd)/include"

  make
}

package() {
  install -Dm754 shutdown-wrapper "$pkgdir/usr/lib/vdr/bin/shutdown-wrapper"
  install -Dm755 shutdown.sh "$pkgdir/usr/lib/vdr/bin/shutdown.sh"

  cd "${srcdir}/${pkgname}-${pkgver}"

  CFLAGS+=' -O3'
  CXXFLAGS+=' -O3 -fPIC'

  make DESTDIR="${pkgdir}" install
}
