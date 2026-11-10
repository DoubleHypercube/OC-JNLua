project('OC-Natives', 'c')
compiler = meson.get_compiler('c')

lua52_sources = [
	'lua52/src/lapi.c',
	'lua52/src/lauxlib.c',
	'lua52/src/lbaselib.c',
	'lua52/src/lbitlib.c',
	'lua52/src/lcode.c',
	'lua52/src/lcorolib.c',
	'lua52/src/lctype.c',
	'lua52/src/ldblib.c',
	'lua52/src/ldebug.c',
	'lua52/src/ldo.c',
	'lua52/src/ldump.c',
	'lua52/src/lfunc.c',
	'lua52/src/lgc.c',
#	'lua52/src/linit.c',
#	'lua52/src/liolib.c',
	'lua52/src/llex.c',
	'lua52/src/lmathlib.c',
	'lua52/src/lmem.c',
#	'lua52/src/loadlib.c',
	'lua52/src/lobject.c',
	'lua52/src/lopcodes.c',
	'lua52/src/loslib.c',
	'lua52/src/lparser.c',
	'lua52/src/lstate.c',
	'lua52/src/lstring.c',
	'lua52/src/lstrlib.c',
	'lua52/src/ltable.c',
	'lua52/src/ltablib.c',
	'lua52/src/ltm.c',
	'lua52/src/lundump.c',
	'lua52/src/lvm.c',
	'lua52/src/lzio.c'
]

lua53_sources = [
	'lua53/src/lapi.c',
	'lua53/src/lauxlib.c',
	'lua53/src/lbaselib.c',
	'lua53/src/lbitlib.c',
	'lua53/src/lcode.c',
	'lua53/src/lcorolib.c',
	'lua53/src/lctype.c',
	'lua53/src/ldblib.c',
	'lua53/src/ldebug.c',
	'lua53/src/ldo.c',
	'lua53/src/ldump.c',
	'lua53/src/lfunc.c',
	'lua53/src/lgc.c',
#	'lua53/src/linit.c',
#	'lua53/src/liolib.c',
	'lua53/src/llex.c',
	'lua53/src/lmathlib.c',
	'lua53/src/lmem.c',
#	'lua53/src/loadlib.c',
	'lua53/src/lobject.c',
	'lua53/src/lopcodes.c',
	'lua53/src/loslib.c',
	'lua53/src/lparser.c',
	'lua53/src/lstate.c',
	'lua53/src/lstring.c',
	'lua53/src/lstrlib.c',
	'lua53/src/ltable.c',
	'lua53/src/ltablib.c',
	'lua53/src/ltm.c',
	'lua53/src/lundump.c',
	'lua53/src/lutf8lib.c',
	'lua53/src/lvm.c',
	'lua53/src/lzio.c'
]

lua54_sources = [
	'lua54/src/lapi.c',
	'lua54/src/lauxlib.c',
	'lua54/src/lbaselib.c',
	'lua54/src/lcode.c',
	'lua54/src/lcorolib.c',
	'lua54/src/lctype.c',
	'lua54/src/ldblib.c',
	'lua54/src/ldebug.c',
	'lua54/src/ldo.c',
	'lua54/src/ldump.c',
	'lua54/src/lfunc.c',
	'lua54/src/lgc.c',
#	'lua54/src/linit.c',
#	'lua54/src/liolib.c',
	'lua54/src/llex.c',
	'lua54/src/lmathlib.c',
	'lua54/src/lmem.c',
#	'lua54/src/loadlib.c',
	'lua54/src/lobject.c',
	'lua54/src/lopcodes.c',
	'lua54/src/loslib.c',
	'lua54/src/lparser.c',
	'lua54/src/lstate.c',
	'lua54/src/lstring.c',
	'lua54/src/lstrlib.c',
	'lua54/src/ltable.c',
	'lua54/src/ltablib.c',
	'lua54/src/ltm.c',
	'lua54/src/lundump.c',
	'lua54/src/lutf8lib.c',
	'lua54/src/lvm.c',
	'lua54/src/lzio.c'
]

lua52_sources += ['lua52/src/eris.c']
lua53_sources += ['lua53/src/eris.c']
lua54_sources += ['lua54/src/eris.c']

# detect lua version
# lua_version_code = '''#include <stdio.h>
# #include "lua.h"
# int main(void) {
#	printf("%d\n", LUA_VERSION_NUM);
#	return 0;
# }'''
# lua_version_result =  compiler.run(lua_version_code, name: 'lua version check', include_directories: incdirs)
# lua_version = lua_version_result.stdout().strip()
#
# if lua_version == '503'
#	jnlua_sources = lua53_sources
# elif lua_version == '502'
#	jnlua_sources = lua52_sources
# else
#	error('Unrecognized Lua version: ' + lua_version)
# endif

jnlua_sources = ['src/jnlua.c']
if get_option('enable_javavm')
	jnlua_sources += ['src/javavm.c']
endif
jnlua_sources += ['src/liolib_none.c']
jnlua_sources += ['src/loadlib_none.c']

# see xkcd 927
add_project_arguments('-std=gnu99', language: 'c')
add_project_arguments('-Wno-error=incompatible-pointer-types', language: 'c')

add_project_arguments('-fno-strict-aliasing', language: 'c')
add_project_arguments('-D_REENTRANT', language: 'c')
if host_machine.system() == 'linux'
	add_project_arguments('-DLUA_USE_LINUX', language: 'c')
elif host_machine.system() == 'darwin'
	add_project_arguments('-DLUA_USE_MACOSX', language: 'c')
elif host_machine.system() == 'windows'
	add_project_arguments('-DLUA_WIN', language: 'c')
else
	add_project_arguments('-DLUA_USE_POSIX', language: 'c')
endif
if get_option('buildtype').startswith('release')
	add_project_arguments('-DNDEBUG', language: 'c')
endif

lib_suffix = '-' + host_machine.system() + '-' + host_machine.cpu_family()

fs = import('fs')
math_dep = compiler.find_library('m', required: true)
if get_option('use_local_jni')
	# try JAVA_HOME first then locate
	java_home = run_command('sh', '-c',
		'if [ -n "$JAVA_HOME" ]; then printf %s "$JAVA_HOME"; ' +
		'elif command -v javac >/dev/null 2>&1; then ' +
		'dirname "$(dirname "$(readlink -f "$(command -v javac)")")"; fi',
		check: false).stdout().strip()
	if java_home == '' or not fs.exists(java_home / 'include' / 'jni.h')
		error('Could not locate a JDK. Try setting JAVA_HOME?')
	endif
	jni_dep = declare_dependency(
		link_args: ['-L' + java_home / 'lib' / 'server', '-ljvm'],
		include_directories: include_directories(
			java_home / 'include',
			java_home / 'include' / host_machine.system(),
			is_system: true
		)
	)
else
	jni_include_dir = join_paths('jni', host_machine.system(), host_machine.cpu_family(), 'include')
	jni_lib_dir = join_paths(meson.source_root(), 'jni', host_machine.system(), host_machine.cpu_family(), 'lib')
	if host_machine.system() == 'windows'
		jni_lib_file = join_paths(jni_lib_dir, 'jvm.dll')
		jni_dep = declare_dependency(
			link_args: [jni_lib_file],
			include_directories: include_directories(jni_include_dir)
		)
	else
		jni_dep = declare_dependency(
			link_args: ['-L' + jni_lib_dir, '-ljvm'],
			include_directories: include_directories(jni_include_dir)
		)
	endif
endif

built_any = false
foreach v : ['52', '53', '54']
	if fs.is_dir('lua' + v + '/src')
		srcs = get_variable('lua' + v + '_sources') + jnlua_sources
		library('jnlua' + v + lib_suffix, srcs, dependencies: [math_dep, jni_dep],
			include_directories: include_directories('lua' + v + '/src', 'src'))
		built_any = true
	else
		warning('lua' + v + '/src is missing, skipping jnlua' + v)
	endif
endforeach
if not built_any
	error('No lua*/src directories found; run: git submodule update --init --recursive')
endif
