AC_INIT([Photos],
        [3.35.1],
        [https://gitlab.gnome.org/GNOME/gnome-photos/issues],
        [gnome-photos],
        [https://wiki.gnome.org/Apps/Photos])
AC_PREREQ(2.63)

AC_CONFIG_AUX_DIR([config])
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_SRCDIR([src])

m4_ifdef([AM_SILENT_RULES],[AM_SILENT_RULES([yes])])

AM_INIT_AUTOMAKE([dist-xz foreign no-dist-gzip serial-tests tar-ustar])
AM_MAINTAINER_MODE([enable])

AX_IS_RELEASE([git-directory])

AX_COMPILER_FLAGS([WARN_CFLAGS],
                  [WARN_LDFLAGS],
                  [$ax_is_release],
                  [],
                  [-Wno-cast-function-type -Wno-error=cast-function-type])

AS_IF([test "x$ax_is_release" != "xyes"], [PACKAGE_COMMIT_ID=$(git describe)])
AS_IF([test -n "$PACKAGE_COMMIT_ID"],
      [AC_DEFINE_UNQUOTED([PACKAGE_COMMIT_ID],
                          ["$PACKAGE_COMMIT_ID"],
                          [Define to the description of this package's latest commit.])])

AC_PROG_CC
AC_PROG_CC_C_O
AC_HEADER_STDC

AC_CHECK_LIBM
AC_SUBST(LIBM)

GLIB_GSETTINGS

CAIRO_MIN_VERSION=1.14.0
DAZZLE_MIN_VERSION=3.26.0
GDATA_MIN_VERSION=0.15.2
GDK_PIXBUF_MIN_VERSION=2.36.8
GEGL_MIN_VERSION=0.4.0
GEXIV2_MIN_VERSION=0.10.8
GFBGRAPH_MIN_VERSION=0.2.1
GLIB_MIN_VERSION=2.57.2
GOA_MIN_VERSION=3.8.0
GRILO_MIN_VERSION=0.3.5
GTK_MIN_VERSION=3.22.16

G_LOG_DOMAIN=AC_PACKAGE_TARNAME
AC_DEFINE_UNQUOTED([G_LOG_DOMAIN], ["$G_LOG_DOMAIN"], [Log domain])

GETTEXT_PACKAGE=AC_PACKAGE_TARNAME
AC_SUBST(GETTEXT_PACKAGE)
AC_DEFINE_UNQUOTED([GETTEXT_PACKAGE], ["$GETTEXT_PACKAGE"],
                   [GETTEXT package name])

AX_REQUIRE_DEFINED([AM_GNU_GETTEXT_VERSION])
AM_GNU_GETTEXT_VERSION([0.19.8])

AX_REQUIRE_DEFINED([AM_GNU_GETTEXT])
AM_GNU_GETTEXT([external])

AC_DEFINE([PHOTOS_EPSILON], [1e-5], [Tolerance when comparing single or double precision floating point values])

LT_INIT

# ****************************************************************
# Support for nl_langinfo (_NL_MEASUREMENT_MEASUREMENT) (optional)
# ****************************************************************
AC_MSG_CHECKING([for _NL_MEASUREMENT_MEASUREMENT])
AC_TRY_LINK([#include <langinfo.h>], [
char c;
c = *((unsigned char *)  nl_langinfo(_NL_MEASUREMENT_MEASUREMENT));
], have_langinfo_measurement=yes, have_langinfo_measurement=no)
AC_MSG_RESULT($have_langinfo_measurement)
if test "$have_langinfo_measurement" = "yes"; then
  AC_DEFINE([HAVE__NL_MEASUREMENT_MEASUREMENT], [1],
            [Define if _NL_MEASUREMENT_MEASUREMENT is available])
fi

PKG_CHECK_MODULES(BABL, [babl])
PKG_CHECK_MODULES(CAIRO, [cairo >= $CAIRO_MIN_VERSION cairo-gobject])
PKG_CHECK_MODULES(DAZZLE, [libdazzle-1.0 >= $DAZZLE_MIN_VERSION])
PKG_CHECK_MODULES(GFBGRAPH, [libgfbgraph-0.2 >= $GFBGRAPH_MIN_VERSION])
PKG_CHECK_MODULES(GDATA, [libgdata >= $GDATA_MIN_VERSION])
PKG_CHECK_MODULES(GEGL, [gegl-0.4 >= $GEGL_MIN_VERSION])
PKG_CHECK_MODULES(GEOCODE, [geocode-glib-1.0])
PKG_CHECK_MODULES(GEXIV2, [gexiv2 >= $GEXIV2_MIN_VERSION])
PKG_CHECK_MODULES(GDK_PIXBUF, [gdk-pixbuf-2.0 >= $GDK_PIXBUF_MIN_VERSION])
PKG_CHECK_MODULES(GLIB, [glib-2.0 >= $GLIB_MIN_VERSION])
PKG_CHECK_MODULES(GIO, [gio-2.0 gio-unix-2.0])

PKG_CHECK_MODULES(GOA, [goa-1.0 >= $GOA_MIN_VERSION])
AC_DEFINE([GOA_API_IS_SUBJECT_TO_CHANGE], [], [We are aware that GOA's API can change])

PKG_CHECK_MODULES(GOBJECT, [gobject-2.0])
PKG_CHECK_MODULES(GRILO, [grilo-0.3 >= $GRILO_MIN_VERSION])
PKG_CHECK_MODULES(GSETTINGS_DESKTOP_SCHEMAS, [gsettings-desktop-schemas])
PKG_CHECK_MODULES(GTK, [gtk+-3.0 >= $GTK_MIN_VERSION])
PKG_CHECK_MODULES(GTK_UNIX_PRINT, [gtk+-unix-print-3.0])
PKG_CHECK_MODULES(JPEG, [libjpeg])
PKG_CHECK_MODULES(PNG, [libpng16])
PKG_CHECK_MODULES(TRACKER, [tracker-control-2.0 tracker-sparql-2.0])

LIBGD_INIT([
  gtk-hacks
  main-box
  main-icon-view
  static
  tagged-entry
][subprojects/libgd])

AC_ARG_ENABLE(dogtail,
              [AS_HELP_STRING([--enable-dogtail],
                              [test using dogtail [default=yes]])],,
              enable_dogtail=yes)
AM_CONDITIONAL(ENABLE_DOGTAIL, test "$enable_dogtail" != no)

GLIB_TESTS

GLIB_COMPILE_RESOURCES=`$PKG_CONFIG --variable glib_compile_resources gio-2.0`
AC_SUBST(GLIB_COMPILE_RESOURCES)

AC_ARG_VAR([DESKTOP_FILE_VALIDATE], [the desktop-file-validate program])
AC_PATH_PROG([DESKTOP_FILE_VALIDATE], [desktop-file-validate], [])
if test -z "$DESKTOP_FILE_VALIDATE"; then
  AC_MSG_ERROR([desktop-file-validate not found])
fi

#############################################
#Documentation
#############################################

AX_REQUIRE_DEFINED([YELP_HELP_INIT])
YELP_HELP_INIT

AC_CONFIG_FILES([
Makefile
data/Makefile
data/org.gnome.Photos.desktop.in
data/icons/Makefile
help/Makefile
subprojects/libgd/Makefile
src/Makefile
tests/Makefile
tests/unit/Makefile
po/Makefile.in
])
AC_OUTPUT
