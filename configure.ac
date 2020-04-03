#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.63])
AC_INIT([idutils], m4_esyscmd([build-aux/git-version-gen .tarball-version]),
	[bug-idutils@gnu.org])

AC_CONFIG_AUX_DIR(build-aux)
AM_INIT_AUTOMAKE([1.11 no-dist-gzip dist-xz color-tests parallel-tests
                  std-options])
AM_SILENT_RULES([yes]) # make --enable-silent-rules the default.

AC_CONFIG_SRCDIR([src/mkid.c])
AC_CONFIG_HEADERS([lib/config.h:lib/config.hin])

# Checks for programs.
AM_MISSING_PROG(HELP2MAN, help2man)
AC_PROG_CC_STDC
AM_PROG_CC_C_O
AC_PROG_LN_S
gl_EARLY
gl_INIT

AC_ARG_ENABLE([gcc-warnings],
  [AS_HELP_STRING([--enable-gcc-warnings],
		  [turn on lots of GCC warnings (for developers)])],
  [case $enableval in
     yes|no) ;;
     *)      AC_MSG_ERROR([bad value $enableval for gcc-warnings option]) ;;
   esac
   gl_gcc_warnings=$enableval],
  [if test -d "$srcdir"/.git; then
     gl_gcc_warnings=yes
   else
     gl_gcc_warnings=no
   fi]
)

if test "$gl_gcc_warnings" = yes; then
  gl_WARN_ADD([-Werror], [WERROR_CFLAGS])
  AC_SUBST([WERROR_CFLAGS])

  # Add many warnings, except some...
  nw=
  nw="$nw -Wduplicated-branches"    # Too many false alarms
  nw="$nw -Wformat-nonliteral"      # who.c and pinky.c strftime uses
  nw="$nw -Wvla"                    # warnings in gettext.h
  nw="$nw -Winline"
  # things I might fix soon:

  gl_MANYWARN_ALL_GCC([ws])
  gl_MANYWARN_COMPLEMENT([ws], [$ws], [$nw])
  for w in $ws; do
    gl_WARN_ADD([$w])
  done
  gl_WARN_ADD([-Wno-missing-field-initializers]) # We need this one
  gl_WARN_ADD([-Wno-sign-compare])     # Too many warnings for now
  gl_WARN_ADD([-Wno-pointer-sign])     # Too many warnings for now
  gl_WARN_ADD([-Wno-unused-parameter]) # Too many warnings for now

  AC_SUBST([WARN_CFLAGS])

  AC_DEFINE([lint], [1], [Define to 1 if the compiler is checking for lint.])
  AC_DEFINE([_FORTIFY_SOURCE], [2],
    [enable compile-time and run-time bounds-checking, and some warnings])
  AC_DEFINE([GNULIB_PORTCHECK], [1], [enable some gnulib portability checks])
fi

AC_PROG_INSTALL
AC_PROG_RANLIB

# check functions

# if HAVE_LINK, then in the code we look for file aliases
# if HAVE_SBRK, then we can generate statistics on memory usage

AC_CHECK_FUNCS([link sbrk lstat])

AM_PATH_LISPDIR

# Checks for header files.

AC_CHECK_HEADERS([termios.h sys/ioctl.h termio.h sgtty.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_C_CONST
AC_C_INLINE
AC_TYPE_OFF_T
AC_TYPE_SIZE_T
AC_CHECK_TYPES(ptrdiff_t)

AM_GNU_GETTEXT([external], [need-ngettext])
AM_GNU_GETTEXT_VERSION([0.18])

AC_CONFIG_FILES([
                 po/Makefile.in
                 doc/Makefile
                 man/Makefile
                 lib/Makefile
                 libidu/Makefile
                 lisp/Makefile
                 testsuite/Makefile
                 gnulib-tests/Makefile
                 src/Makefile
		 Makefile
                ])
AC_OUTPUT
