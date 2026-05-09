project('reflect-cpp', ['c', 'cpp'],
        version : '0.24.0',
        default_options : [
            'cpp_std=c++20',
            'c_std=c11',
            'warning_level=0',
            'werror=false',
        ]
)

inc          = include_directories('include',                is_system : true)
inc_3rdparty = include_directories('include/rfl/thirdparty', is_system : true)

sources = files('src/reflectcpp.cpp')
deps    = []

# JSON — enabled by default, uses bundled yyjson
if get_option('json').enabled()
    sources += files('src/reflectcpp_json.cpp', 'src/yyjson.c')
endif

# CBOR and UBJSON share the jsoncons dependency
if get_option('cbor').enabled() or get_option('ubjson').enabled()
    jsoncons_dep = dependency('jsoncons', required : true)
    deps += jsoncons_dep
    if get_option('cbor').enabled()
        sources += files('src/reflectcpp_cbor.cpp')
    endif
    if get_option('ubjson').enabled()
        sources += files('src/reflectcpp_ubjson.cpp')
    endif
endif

if get_option('msgpack').enabled()
    deps    += dependency('msgpack', required : true)
    sources += files('src/reflectcpp_msgpack.cpp')
endif

if get_option('toml').enabled()
    deps    += dependency('tomlplusplus', required : true)
    sources += files('src/reflectcpp_toml.cpp')
endif

if get_option('xml').enabled()
    deps    += dependency('pugixml', required : true)
    sources += files('src/reflectcpp_xml.cpp')
endif

if get_option('yaml').enabled()
    deps    += dependency('yaml-cpp', required : true)
    sources += files('src/reflectcpp_yaml.cpp')
endif

if get_option('cereal').enabled()
    deps    += dependency('cereal', required : true)
    sources += files('src/reflectcpp_cereal.cpp')
endif

if get_option('flexbuffers').enabled()
    deps    += dependency('flatbuffers', required : true)
    sources += files('src/reflectcpp_flexbuf.cpp')
endif

if get_option('boost_serialization').enabled()
    deps    += dependency('boost', modules : ['serialization'], required : true)
    sources += files('src/reflectcpp_boost_serialization.cpp')
endif

reflectcpp_lib = static_library('reflectcpp',
    sources,
    include_directories : [inc, inc_3rdparty],
    dependencies        : deps,
)

reflect_dep = declare_dependency(
    include_directories : inc,
    link_with           : reflectcpp_lib,
    dependencies        : deps,
)
