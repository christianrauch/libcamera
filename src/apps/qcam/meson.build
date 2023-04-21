# SPDX-License-Identifier: CC0-1.0

qt = import('qt6')
qt_dep = dependency('qt6',
                    modules : ['Core', 'Gui', 'Widgets', 'OpenGL', 'OpenGLWidgets'],
                    required : get_option('qcam'),
                    version : '>=6.2')

if not qt_dep.found()
    qcam_enabled = false
    subdir_done()
endif

qcam_enabled = true

qcam_sources = files([
    'cam_select_dialog.cpp',
    'format_converter.cpp',
    'main.cpp',
    'main_window.cpp',
    'message_handler.cpp',
    'viewfinder_qt.cpp',
])

qcam_moc_headers = files([
    'cam_select_dialog.h',
    'main_window.h',
    'viewfinder_qt.h',
])

qcam_resources = files([
    'assets/feathericons/feathericons.qrc',
])

qt_cpp_args = [apps_cpp_args, '-DQT_NO_KEYWORDS']

if cxx.has_header_symbol('QOpenGLWidget', 'QOpenGLWidget',
                         dependencies : qt_dep, args : '-fPIC')
    qcam_sources += files([
        'viewfinder_gl.cpp',
    ])
    qcam_moc_headers += files([
        'viewfinder_gl.h',
    ])
    qcam_resources += files([
        'assets/shader/shaders.qrc'
    ])
endif

resources = qt.preprocess(moc_headers : qcam_moc_headers,
                          qresources : qcam_resources,
                          dependencies : qt_dep)

qcam  = executable('qcam', qcam_sources, resources,
                   install : true,
                   install_tag : 'bin',
                   link_with : apps_lib,
                   dependencies : [
                       libatomic,
                       libcamera_public,
                       libtiff,
                       qt_dep,
                   ],
                   cpp_args : qt_cpp_args)
