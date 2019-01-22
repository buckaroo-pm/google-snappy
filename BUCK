load('//:buckaroo_macros.bzl', 'buckaroo_deps_from_package')
load('//:subdir_glob.bzl', 'subdir_glob')
load('//:extract.bzl', 'extract')

genrule(
  name = 'cmake',
  out = 'out',
  srcs = glob([
    'cmake/*.in',
    'cmake/*.cmake',
    '*.in',
    '*.txt',
    '*.h',
    '*.cc',
  ]),
  cmd = ' && '.join([
    'mkdir -p $OUT',
    'cd $OUT',
    'cmake -DSNAPPY_BUILD_TESTS=OFF $SRCDIR',
  ]),
)

config = extract(':cmake', 'config.h')

zlib = buckaroo_deps_from_package('github.com/buckaroo-pm/madler-zlib')

cxx_library(
  name = 'snappy',
  header_namespace = '',
  exported_headers = [
    'snappy.h',
    extract(':cmake', 'snappy-stubs-public.h'),
  ],
  headers = [
    'snappy-stubs-internal.h',
    'snappy-internal.h',
    'snappy-sinksource.h',
    config,
  ],
  licenses = [
    'COPYING',
  ],
  srcs = [
    'snappy.cc',
    'snappy-stubs-internal.cc',
    'snappy-sinksource.cc',
  ],
  deps = zlib,
  visibility = [
    'PUBLIC',
  ],
)

gtest = buckaroo_deps_from_package('github.com/buckaroo-pm/google-googletest')

cxx_test(
  name = 'snappy-test',
  headers = [
    'snappy-test.h',
    'snappy-stubs-internal.h',
    config,
  ],
  srcs = [
    'snappy_unittest.cc',
    'snappy-test.cc',
  ],
  deps = [
    ':snappy',
  ] + gtest,
)
