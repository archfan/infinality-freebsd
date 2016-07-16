# Created by: Joe Marcus Clarke <marcus@FreeBSD.org>
# $FreeBSD: branches/2016Q3/x11-fonts/fontconfig/Makefile 414887 2016-05-09 19:18:14Z sunpoet $

PORTNAME=	fontconfig
PORTVERSION=	2.11.95
PORTREVISION?=	5
PORTEPOCH?=	1
CATEGORIES=	x11-fonts
MASTER_SITES=	http://www.freedesktop.org/software/fontconfig/release/

PATCH_SITES=    https://github.com/bohoomil/fontconfig-ultimate/raw/master/fontconfig_patches/
PATCHFILES=     01-configure.patch:-p1 02-configure.ac.patch:-p1 03-Makefile.in.patch:-p1 04-Makefile.conf.d.patch:-p1 05-Makefile.am.in.patch:-p1 

post-patch:
		@${MKDIR} ${WRKSRC}/conf.d.infinality/
		@${CP} ${FILESDIR}/conf.d.infinality/* ${WRKSRC}/conf.d.infinality/

MAINTAINER=	gnome@FreeBSD.org
COMMENT=	XML-based font configuration API for X Windows

USES=		tar:bzip2

.if !defined(REFERENCE_PORT)

LIB_DEPENDS=	libfreetype.so:print/freetype2 \
		libexpat.so:textproc/expat2

USES+=		gmake libtool pathfix pkgconfig autoreconf
USE_LDCONFIG=	yes
GNU_CONFIGURE=	yes
CONFIGURE_ARGS=	--with-configdir=${PREFIX}/etc/fonts/conf.d \
		--with-templatedir=${PREFIX}/etc/fonts/conf.avail \
		--with-templateinfdir=${PREFIX}/etc/fonts/conf.avail.infinality \
		--with-xmldir=${PREFIX}/etc/fonts \
		--with-expat-includes=${LOCALBASE}/include \
		--with-expat-lib=${LOCALBASE}/lib \
		--with-default-fonts=${PREFIX}/share/fonts \
		--with-add-fonts=${LOCALBASE}/lib/X11/fonts \
		--with-cache-dir=/var/db/fontconfig \
		--with-pkgconfigdir=${PREFIX}/libdata/pkgconfig \
		--disable-docs 

# D6155: static linked library linking added to support statically linked
# 	 applications, e.g. x11/xforward static option
CONFIGURE_ARGS+= --enable-static
#CONFIGURE_ARGS+= 


MAKE_ENV=	LC_ALL=C
PKGINSTALL=	${WRKDIR}/pkg-install
INSTALL_TARGET=install-strip

MAN1PAGES=	fc-cache.1 fc-cat.1 fc-list.1 fc-match.1 fc-query.1 fc-scan.1
MAN5PAGES=	fonts-conf.5
PORTDOCS=	fontconfig-user.html fontconfig-user.pdf fontconfig-user.txt

OPTIONS_DEFINE=	DOCS

post-install:
	${INSTALL_MAN} ${MAN5PAGES:S|^|${WRKSRC}/doc/|} \
		${STAGEDIR}${PREFIX}/man/man5
.for manpage in ${MAN1PAGES}
	${INSTALL_MAN} ${FILESDIR}/${manpage} \
		${STAGEDIR}${PREFIX}/man/man1
.endfor
	${INSTALL_DATA} ${WRKSRC}/fonts.conf \
		${STAGEDIR}${PREFIX}/etc/fonts/fonts.conf.sample
	${INSTALL_DATA} ${FILESDIR}/fc-presets \
		${STAGEDIR}${PREFIX}/bin/
	@${SED} -e 's|%%PREFIX%%|${PREFIX}| ; \
		s|%%LOCALBASE%%|${LOCALBASE}|' < ${FILESDIR}/pkg-install.in \
		> ${PKGINSTALL}

post-install-DOCS-on:
	@${MKDIR} ${STAGEDIR}${DOCSDIR}
	${INSTALL_DATA} ${PORTDOCS:S|^|${WRKSRC}/doc/|} ${STAGEDIR}${DOCSDIR}

.include <bsd.port.mk>

.endif
