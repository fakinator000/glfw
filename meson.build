
project('glfw','c', 'cpp', default_options: ['c_std=c11', 'cpp_std=c++17'])

glfw_conf = configuration_data()

linux_deps = dependency('', required: false)
thread_dep = dependency('threads')

link_libdl = ''

cc = meson.get_compiler('c')
m_dep = cc.find_library('m', required: false)

if host_machine.system()  == 'linux'
  glfw_src = files(
	'src/context.c',
	'src/egl_context.c',
	'src/init.c',
	'src/input.c',
	'src/monitor.c',
	'src/vulkan.c',
	'src/window.c',
	
	'src/x11_init.c',
	'src/x11_monitor.c',
	'src/x11_window.c',
	'src/xkb_unicode.c',
	'src/posix_time.c',
	'src/posix_thread.c',
	'src/glx_context.c',
	'src/egl_context.c',
	'src/osmesa_context.c',
	'src/linux_joystick.c')

  link_libdl = '-ldl'

  if get_option('glfw_linux_wayland')
    glfw_conf.set('_GLFW_WAYLAND', true)
  elif get_option('glfw_linux_osmesa')
    glfw_conf.set('_GLFW_OSMESA', true)
  else
    glfw_conf.set('_GLFW_X11', true)

    linux_deps = [
      dependency('xi'),
      dependency('xrandr'),
      dependency('gl'),
      dependency('x11')
    ]
  endif
elif host_machine.system() == 'windows'
  glfw_src = files(
	'src/context.c',
	'src/egl_context.c',
	'src/init.c',
	'src/input.c',
	'src/monitor.c',
	'src/vulkan.c',
	'src/window.c',

	'src/win32_init.c',
	'src/win32_joystick.c',
	'src/win32_monitor.c',
	'src/win32_time.c',
	'src/win32_thread.c',
	'src/win32_window.c',
	'src/wgl_context.c',
	'src/egl_context.c',
	'src/osmesa_context.c')
  glfw_conf.set('_GLFW_WIN32', true)
endif

glfw_inc = include_directories('include')

subdir('src')

libglfw = shared_library(
  'glfw', 
  glfw_src,
  include_directories: [glfw_inc, glfw_conf_inc], 
  dependencies: [linux_deps, thread_dep, m_dep],
  link_args: [link_libdl]
)

