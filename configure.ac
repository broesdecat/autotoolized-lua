AC_PREREQ(2.59)
AC_INIT([Autotoolized Lua], [5.1.4], [], [autotolized-lua])

AC_CONFIG_HEADERS([config.h])
AC_CONFIG_SRCDIR([src/lapi.c])

AM_INIT_AUTOMAKE([1.9 foreign])

AC_PROG_CC
AC_PROG_LIBTOOL

LUA_LIBS="-lm"

# Check for readline
READLINE_DEFS="#undef LUA_USE_READLINE"
AC_SUBST(READLINE_DEFS)

case "$host" in
  *-mingw*)  use_os=win32  ;;
  *-darwin*) use_os=macosx ;;
  *)         use_os=posix  ;;
esac

echo "$use_os"

POSIX_DEFS="#undef LUA_USE_POSIX"
LUA_DL_DEFS="#undef LUA_USE_DLOPEN"
LUA_BUILD_AS_DLL_DEFS="#undef LUA_BUILD_AS_DLL"

if test "x$use_os" == "xwin32"; then
  LUA_BUILD_AS_DLL_DEFS="#define LUA_BUILD_AS_DLL"
fi
AC_SUBST(POSIX_DEFS)
AC_SUBST(LUA_DL_DEFS)
AC_SUBST(LUA_BUILD_AS_DLL_DEFS)

AC_SUBST(LUA_LIBS)

AC_CONFIG_FILES([Makefile
                 src/Makefile
                 src/luaconf.h.template
                 etc/Makefile
                 etc/lua.pc
                 doc/Makefile
		 test/Makefile])
AC_OUTPUT
