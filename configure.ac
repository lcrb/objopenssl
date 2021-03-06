AC_INIT(ObjOpenSSL, 0.1, js@heap.zone)

AC_CONFIG_SRCDIR(src)
AC_CONFIG_AUX_DIR(build-aux)
AC_CONFIG_MACRO_DIR(build-aux/m4)

AC_CHECK_TOOL(OBJFW_CONFIG, objfw-config)
AS_IF([test x"$OBJFW_CONFIG" = x""], [
       AC_MSG_ERROR(You need ObjFW and objfw-config installed!)
])

test x"$OBJC" = x"" && OBJC="$($OBJFW_CONFIG --objc)"

AC_LANG([Objective C])
AC_PROG_OBJC
AC_PROG_OBJCPP
AC_PROG_LN_S
AC_PROG_INSTALL

CPP="$OBJCPP"
CPPFLAGS="$CPPFLAGS $OBJCPPFLAGS $($OBJFW_CONFIG --cppflags)"
OBJCFLAGS="$OBJCFLAGS -Wall $($OBJFW_CONFIG --objcflags)"
LDFLAGS="$LDFLAGS $($OBJFW_CONFIG --ldflags --rpath)"
LIBS="$LIBS $($OBJFW_CONFIG --libs)"

AX_CHECK_COMPILER_FLAGS(-Wdocumentation,
	[OBJCFLAGS="$OBJCFLAGS -Wdocumentation"])

AC_ARG_ENABLE(shared,
	AS_HELP_STRING([--disable-shared], [do not build shared library]))
AS_IF([test x"$enable_shared" != x"no"], [
	BUILDSYS_SHARED_LIB
	AC_SUBST(OBJOPENSSL_SHARED_LIB, "${LIB_PREFIX}objopenssl${LIB_SUFFIX}")
])
AC_ARG_ENABLE(static, AS_HELP_STRING([--enable-static], [build static library]))
AS_IF([test x"$enable_static" = x"yes" -o x"$enable_shared" = x"no"], [
	AC_PATH_TOOL(AR, ar)
	AC_PROG_RANLIB
	AC_SUBST(OBJOPENSSL_STATIC_LIB, "libobjopenssl.a")
])

PKG_CHECK_MODULES(openssl, [openssl >= 1.0.2], [
	CPPFLAGS="$CPPFLAGS $openssl_CFLAGS"
	LIBS="$LIBS $openssl_LIBS"
], [
	AC_MSG_ERROR(You need openssl >= 1.0.2 installed!)
])

AS_IF([test x"$GOBJC" = x"yes"], [
	OBJCFLAGS="$OBJCFLAGS -Wwrite-strings -Wpointer-arith -Werror"
])

BUILDSYS_INIT

dnl We don't call AC_PROG_CPP, but only AC_PROG_OBJCPP and set CPP to OBJCPP
dnl and add OBJCPPFLAGS to CPPFLAGS, thus we need to AC_SUBST these ourself.
AC_SUBST(CPP)
AC_SUBST(CPPFLAGS)

AC_SUBST(PACKAGE, ObjOpenSSL)
AC_CONFIG_FILES([buildsys.mk extra.mk])
AC_OUTPUT
