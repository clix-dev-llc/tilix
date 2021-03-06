
# install the icons
install_data(
    'icons/hicolor/scalable/apps/@0@.svg'.format(project_id),
    install_dir: iconsdir / 'scalable' / 'apps'
)
install_data(
    'icons/hicolor/scalable/apps/@0@-symbolic.svg'.format(project_id),
    install_dir: iconsdir / 'symbolic' / 'apps'
)

# Install the desktop file
desktop_file = i18n.merge_file(
    '@0@.desktop'.format(project_id),
    output: '@BASENAME@',
    input: 'pkg/desktop/@0@.desktop.in'.format(project_id),
    po_dir: meson.source_root() / 'po',
    type: 'desktop',
    install: true,
    install_dir: appdir
)

# Install manual page
install_man(['man/tilix.1'])

# Validate desktop file
desktop_file_validate = find_program('desktop-file-validate', required:false)
if desktop_file_validate.found()
  test (
    'Validate desktop file',
    desktop_file_validate,
    args: desktop_file.full_path()
  )
endif

# Install the MetaInfo file
metainfo_file = i18n.merge_file(
    '@0@.appdata.xml'.format(project_id),
    output: '@BASENAME@',
    input: 'appdata/@0@.appdata.xml.in'.format(project_id),
    po_dir: meson.source_root() / 'po',
    install: true,
    install_dir: datadir / 'metainfo'
)

# Validate AppData file
appstreamcli = find_program('appstreamcli', required: false)
if appstreamcli.found()
  test (
    'Validate metainfo file',
    appstreamcli,
    args: ['validate', '--nonet', metainfo_file.full_path()]
  )
endif

# Build resources
gresource = gnome.compile_resources(
    'tilix',
    'resources/tilix.gresource.xml',
    gresource_bundle: true,
    install: true,
    source_dir: 'resources/',
    install_dir: pkgdatadir / 'resources'
)

# Install GSchemas
install_data(
  'gsettings/@0@.gschema.xml'.format(project_id),
  install_dir: schemadir
)

# Color schemes
install_data(
    [
        'schemes/base16-twilight-dark.json',
        'schemes/linux.json',
        'schemes/material.json',
        'schemes/monokai.json',
        'schemes/orchis.json',
        'schemes/solarized-dark.json',
        'schemes/solarized-light.json',
        'schemes/tango.json',
        'schemes/yaru.json',
    ],
    install_dir: pkgdatadir / 'schemes'
)

# Scripts
install_data(
    'scripts/tilix_int.sh',
    install_dir: pkgdatadir / 'scripts'
)

# Copying Nautilus extension
install_data(
    'nautilus/open-tilix.py',
    install_dir: datadir / 'nautilus-python' / 'extensions'
)

# Copy D-Bus service descriptor
conf = configuration_data()
conf.set('bindir', bindir)

configure_file(
  input: 'dbus/@0@.service.in'.format(project_id),
  output: '@BASENAME@',
  configuration: conf,
  install_dir: datadir / 'dbus-1' / 'services'
)
