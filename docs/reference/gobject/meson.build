if get_option('ctk_doc')
  subdir('xml')

  ignore_headers = [
    'tests',
    'gatomicarray.h',
    'gobject_probes.h',
    'gobject_trace.h',
    'gtype-private.h',
    'glib-enumtypes.h',
  ]

  docpath = join_paths(glib_datadir, 'ctk-doc', 'html')
  version_conf = configuration_data()
  version_conf.set('GLIB_VERSION', meson.project_version())
  configure_file(
    input: 'version.xml.in',
    output: 'version.xml',
    configuration: version_conf
  )

  ctkdocincl = include_directories('.')

  gnome.ctkdoc('gobject',
    main_xml : 'gobject-docs.xml',
    namespace : 'g',
    mode : 'none',
    dependencies : [libgobject_dep, libglib_dep],
    include_directories : [ctkdocincl],
    src_dir : 'gobject',
    scan_args : ctkdoc_common_scan_args + [
      '--rebuild-types',
      '--ignore-headers=' + ' '.join(ignore_headers),
    ],
    content_files : [
      'glib-mkenums.xml',
      'glib-genmarshal.xml',
      'gobject-query.xml',
      'tut_gobject.xml',
      'tut_gsignal.xml',
      'tut_gtype.xml',
      'tut_howto.xml',
      'tut_intro.xml',
      'tut_tools.xml'
    ],
    html_assets : [
      'images/glue.png'
    ],
    fixxref_args: [
      '--html-dir=' + docpath,
      '--extra-dir=' + join_paths('gobject', '..', 'glib', 'html'),
    ],
    install: true,
    check: true,
  )
endif

if get_option('man')
  manpages = ['glib-mkenums', 'glib-genmarshal', 'gobject-query']
  foreach page : manpages
    custom_target(page + '-man',
      input: page + '.xml',
      output: page + '.1',
      command: xsltproc_command,
      install: true,
      install_dir: man1_dir)
  endforeach
endif
