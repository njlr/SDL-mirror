# We call CMake to generate the config
# TODO: Port this logic properly
genrule(
  name = 'cmake',
  out = 'out',
  srcs = glob([
    'include/**/*.h',
    'include/**/*.in',
    'include/**/*.cmake',
    'src/**/*.c',
    'cmake/*.cmake',
    '*.txt',
    '*.in',
  ]),
  cmd = 'mkdir $OUT && cd $OUT && cmake $SRCDIR',
)

genrule(
  name = 'SDL_config.h',
  out = 'SDL_config.h',
  cmd = 'cp $(location :cmake)/include/SDL_config.h $OUT',
)

prebuilt_cxx_library(
  name = 'config',
  header_only = True,
  header_namespace = '',
  exported_headers = {
    'SDL_config.h': ':SDL_config.h',
  },
)

cxx_library(
  name = 'atomic',
  header_namespace = '',
  exported_headers = subdir_glob([
    ('include', 'SDL_atomic.h'),
  ]),
  srcs = glob([
    'src/atomic/**/*.c',
  ]),
  deps = [
    ':config',
  ],
)

emscripten_sources = glob([
  'src/filesystem/emscripten/**/*.c',
])

android_sources = glob([
  'src/core/android/**/*.c',
  'src/filesystem/android/**/*.c',
  'src/render/opengles/**/*.c',
])

linux_sources = glob([
  'src/core/linux/**/*.c',
  'src/haptic/linux/**/*.c',
  'src/filesystem/unix/**/*.c',
  'src/thread/pthread/**/*.c',
])

haiku_sources = glob([
  'src/filesystem/haiku/**/*.cc',
])

nacl_sources = glob([
  'src/filesystem/nacl/**/*.c',
])

windows_sources = glob([
  'src/core/windows/**/*.c',
  'src/haptic/windows/**/*.c',
  'src/joystick/windows/**/*.c',
  'src/filesystem/windows/**/*.c',
  'src/thread/windows/**/*.c',
])

winrt_sources = glob([
  'src/core/winrt/**/*.c',
  'src/core/winrt/**/*.cpp',
  'src/filesystem/winrt/**/*.cpp',
])

macos_sources = glob([
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

bsd_sources = glob([
  'src/joystick/bsd/**/*.c',
])

platform_sources = haiku_sources + nacl_sources + emscripten_sources + android_sources + linux_sources + windows_sources + winrt_sources + macos_sources

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
  # '-framework', 'UIKit',
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
  ], excludes = platform_sources),
  platform_srcs = [
    ('default', macos_sources),
    ('^macos.*', macos_sources),
    ('^linux.*', linux_sources),
    ('^windows.*', windows_sources),
    ('^android.*', android_sources),
  ],
  visibility = [
    'PUBLIC',
  ],
  exported_platform_linker_flags = [
    ('default', macos_exported_linker_flags),
    ('^macos.*', macos_exported_linker_flags),
    ('^linux.*', linux_exported_linker_flags),
  ],
  deps = [
    # ':config',
  ],
)
