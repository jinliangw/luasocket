project('lua-socket', 'c', version: '0.1')

# Find LuaJIT dependency. This assumes luajit is installed and discoverable by pkg-config.
# If luajit is not found, you might need to set the PKG_CONFIG_PATH environment variable
# or ensure luajit is installed system-wide or in a location meson can find.
luajit_dep = dependency('luajit', required: true)

# Common compiler and linker flags
common_c_args = ['-fPIC'] # Position-independent code for shared libraries
common_link_args = [
  '-Wl,-O1',              # Optimization level 1
  '-Wl,--hash-style=gnu',  # GNU hash style
  '-Wl,--as-needed',       # Link against shared libraries only if needed
]

# Source files for the modules
socket_c_sources = [
  'src/compat.c',
  'src/luasocket.c',
  'src/timeout.c',
  'src/buffer.c',
  'src/io.c',
  'src/auxiliar.c',
  'src/options.c',
  'src/inet.c',
  'src/usocket.c',
  'src/except.c',
  'src/select.c',
  'src/tcp.c',
  'src/udp.c',
]

mime_c_sources = [
  'src/mime.c',
  'src/compat.c',
]

unix_c_sources = [
  'src/compat.c',
  'src/buffer.c',
  'src/auxiliar.c',
  'src/options.c',
  'src/timeout.c',
  'src/io.c',
  'src/usocket.c',
  'src/unixstream.c',
  'src/unixdgram.c',
  'src/unix.c',
]

serial_c_sources = [
  'src/compat.c',
  'src/buffer.c',
  'src/auxiliar.c',
  'src/options.c',
  'src/timeout.c',
  'src/io.c',
  'src/usocket.c',
  'src/serial.c',
]

# Define installation directories
# Lua sources installation path (e.g., /usr/share/lua/5.1/)
lua_source_module_path = join_paths(get_option('prefix'), 'share', 'lua', '5.1')
# Compiled C Lua modules installation path (e.g., /usr/lib/lua/5.1/)
lua_c_module_path = join_paths(get_option('prefix'), 'lib', 'lua', '5.1')

# Install Lua files
install_data(
    ['src/ltn12.lua', 'src/socket.lua', 'src/mime.lua'],
    install_dir: lua_source_module_path
)

install_data(
    ['src/http.lua', 'src/url.lua', 'src/tp.lua', 'src/ftp.lua', 'src/headers.lua', 'src/smtp.lua'],
    install_dir: join_paths(lua_source_module_path, 'socket')
)

install_data(
    ['src/mime.lua'],
    install_dir: join_paths(lua_source_module_path, 'mime')
)

# Build shared libraries (Lua modules)

# 'socket' module (named 'core' for installation)
socket_mod = shared_library(
  'core',
  socket_c_sources,
  dependencies: [luajit_dep],
  name_prefix: '', # Ensures the output is core.so, not libcore.so
  c_args: common_c_args,
  link_args: common_link_args,
  install: true,
  install_dir: join_paths(lua_c_module_path, 'socket')
)

# 'mime' module (named 'mime_core' to avoid conflict)
mime_mod = shared_library(
  'mime_core',
  mime_c_sources,
  dependencies: [luajit_dep],
  name_prefix: '',
  c_args: common_c_args,
  link_args: common_link_args,
  install: true,
  install_dir: join_paths(lua_c_module_path, 'mime'),
  outfile: 'core.so'
)

# 'unix' module
unix_mod = shared_library(
  'unix',
  unix_c_sources,
  dependencies: [luajit_dep],
  name_prefix: '', # Ensures the output is unix.so, not libunix.so
  c_args: common_c_args,
  link_args: common_link_args,
  install: true,
  install_dir: join_paths(lua_c_module_path, 'socket')
)

# 'serial' module
serial_mod = shared_library(
  'serial',
  serial_c_sources,
  dependencies: [luajit_dep],
  name_prefix: '', # Ensures the output is serial.so, not libserial.so
  c_args: common_c_args,
  link_args: common_link_args,
  install: true,
  install_dir: join_paths(lua_c_module_path, 'socket')
)
