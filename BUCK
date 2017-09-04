cxx_library(
  name = 'atomic',
  header_namespace = '',
  exported_headers = subdir_glob([
    ('include', 'SDL_atomic.h'),
  ]),
  srcs = glob([
    'src/atomic/**/*.c',
  ]),
)

emscripten_srcs = glob([
  'src/filesystem/emscripten/**/*.c',
])

android_srcs = glob([
  'src/core/android/**/*.c',
  'src/filesystem/android/**/*.c',
  'src/render/opengles/**/*.c',
])

linux_srcs = glob([
  'src/core/linux/**/*.c',
  'src/haptic/linux/**/*.c',
  'src/filesystem/unix/**/*.c',
  'src/thread/pthread/**/*.c',
])

haiku_srcs = glob([
  'src/filesystem/haiku/**/*.cc',
])

nacl_srcs = glob([
  'src/filesystem/nacl/**/*.c',
])

windows_srcs = glob([
  'src/core/windows/**/*.c',
  'src/haptic/windows/**/*.c',
  'src/joystick/windows/**/*.c',
  'src/filesystem/windows/**/*.c',
  'src/thread/windows/**/*.c',
])

winrt_srcs = glob([
  'src/core/winrt/**/*.c',
  'src/core/winrt/**/*.cpp',
  'src/filesystem/winrt/**/*.cpp',
])

macos_srcs = glob([
  'src/audio/coreaudio/**/*.m',
  'src/haptic/darwin/**/*.c',
  'src/file/cocoa/**/*.m',
  'src/filesystem/cocoa/**/*.m',
  'src/power/macosx/**/*.c',
  'src/joystick/darwin/**/*.c',
  'src/render/opengl/**/*.c',
  'src/video/cocoa/**/*.m',
  'src/video/uikit/**/*.m',
  'src/thread/pthread/**/*.c',
])

bsd_srcs = glob([
  'src/joystick/bsd/**/*.c',
])

platform_srcs = haiku_srcs + nacl_srcs + emscripten_srcs + android_srcs + \
                linux_srcs + windows_srcs + winrt_srcs + macos_srcs

macos_exported_linker_flags = [
  '-framework', 'CoreAudio',
  '-framework', 'CoreAudioKit',
  '-framework', 'CoreVideo',
  '-framework', 'CoreFoundation',
  '-framework', 'Foundation',
  '-framework', 'Cocoa',
  '-framework', 'Carbon',
  '-framework', 'AppKit',
  '-framework', 'AudioUnit',
  '-framework', 'AudioToolbox',
  '-framework', 'IOKit',
  '-framework', 'ForceFeedback',
]

linux_exported_linker_flags = [
  '-lpthread',
]

cxx_library(
  name = 'sdl2',
  header_namespace = '',
  exported_headers = subdir_glob([
    ('include', '*.h'),
  ]),
  srcs = glob([
    'src/**/*.c',
  ], excludes = platform_srcs),
  platform_srcs = [
    ('default', macos_srcs),
    ('^macos.*', macos_srcs),
    ('^linux.*', linux_srcs),
    ('^windows.*', windows_srcs),
    ('^android.*', android_srcs),
  ],
  exported_platform_linker_flags = [
    ('default', macos_exported_linker_flags),
    ('^macos.*', macos_exported_linker_flags),
    ('^linux.*', linux_exported_linker_flags),
  ],
  visibility = [
    'PUBLIC',
  ],
)
