project('xed', 'c', version : '2.0.2')

gnome = import('gnome')
i18n = import('i18n')
pkgconfig = import('pkgconfig')

version = meson.project_version()

xed_conf = configuration_data()
xed_conf.set_quoted('VERSION', version)
xed_conf.set_quoted('GETTEXT_PACKAGE', 'xed')

# directories
prefix = get_option('prefix')
bindir = get_option('bindir')
datadir = get_option('datadir')
libdir = get_option('libdir')
includedir = get_option('includedir')
libexecdir = get_option('libexecdir')
desktopdir = join_paths(datadir, 'applications')
schema_dir = join_paths(datadir, 'glib-2.0', 'schemas')

po_dir = join_paths(meson.source_root(), 'po')

# dependencies
libxml = dependency('libxml-2.0', version: '>= 2.5.0')
glib = dependency('glib-2.0', version: '>= 2.40.0')
gthread = dependency('gthread-2.0', version: '>= 2.13.0')
gio = dependency('gio-2.0', version: '>= 2.40.0')
gtk = dependency('gtk+-3.0', version: '>= 3.19.3')
gtksourceview = dependency('gtksourceview-3.0', version: '>= 3.19.0')
libpeas = dependency('libpeas-1.0', version: '>= 1.12.0')
libpeas_gtk = dependency('libpeas-gtk-1.0', version: '>= 1.12.0')
gir_dep = dependency('gobject-introspection-1.0', version: '>= 0.6.7', required: false)
gmodule = dependency('gmodule-2.0')
xapp = dependency('xapp')
X11 = dependency('x11')
pango = dependency('pango')

if gir_dep.found()
    xed_conf.set('ENABLE_INTROSPECTION', 1)
endif

enable_spell = get_option('enable_spell')
if enable_spell
    gspell = dependency('gspell-1', version: '>= 0.2.5')
endif

if get_option('enable_gvfs_metadata')
    xed_conf.set('ENABLE_GVFS_METADATA', true)
endif

# on some systems we need to find the math lib to make sure it builds
cc = meson.get_compiler('c')
math = cc.find_library('m', required: false)

intltool_merge = find_program('intltool-merge')

# generate config.h
config_h_file = configure_file(
    output : 'config.h',
    configuration : xed_conf
)

config_h = declare_dependency(
    sources: config_h_file
)

include_dirs = include_directories('.', 'xed')

# compiler flags
add_global_arguments('-DDATADIR="@0@"'.format(join_paths(prefix, datadir)), language: 'c')
add_global_arguments('-DLIBDIR="@0@"'.format(join_paths(prefix, libdir)), language: 'c')

# remove this later or add as build option
if not get_option('deprecated_warnings')
    add_global_arguments([
            '-Wno-deprecated-declarations',
            '-Wno-deprecated',
            '-Wno-declaration-after-statement',
        ],
        language: 'c',
    )
endif

subdir('xed')
subdir('pixmaps')
subdir('po')
subdir('data')
subdir('plugins')
subdir('help')
if get_option('docs')
    subdir('docs/reference')
endif

message('prefix = @0@'.format(prefix))
message('bindir = @0@'.format(bindir))
message('datadir = @0@'.format(datadir))
message('libdir = @0@'.format(libdir))
message('includedir = @0@'.format(includedir))
message('libexecdir = @0@'.format(libexecdir))
message('desktopdir = @0@'.format(desktopdir))
message('schema_dir = @0@'.format(schema_dir))
