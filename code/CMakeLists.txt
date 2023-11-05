fips_begin_lib(sokol)
    fips_files(sokol.c)
    if (FIPS_OSX)
        fips_frameworks_osx(Cocoa QuartzCore Metal MetalKit AudioToolbox)
    elseif (FIPS_LINUX)
        fips_libs(X11 Xi Xcursor GL m dl asound)
    endif()
    if(FIPS_EMSCRIPTEN)
        target_compile_definitions(sokol PRIVATE SOKOL_GLES3)
        set(slang "glsl300es")
    elseif(FIPS_WINDOWS)
        target_compile_definitions(sokol PRIVATE SOKOL_D3D11)
        set(slang "hlsl4")
    elseif(FIPS_OSX)
        target_compile_options(sokol PRIVATE -x objective-c)
        target_compile_definitions(sokol PRIVATE SOKOL_METAL)
        set(slang "metal_macos")
    else()
        target_compile_definitions(sokol PRIVATE SOKOL_GLCORE33)
        set(slang "glsl330")
    endif()
fips_end_lib()

fips_begin_app(tech42 windowed)
    fips_files(
        main.c
    )
    fips_deps(sokol)
    # sokol_shader(sokol_shaders.glsl ${slang})
    # fipsutil_copy(assets.yml)
    if (FIPS_CLANG OR FIPS_GCC)
        target_compile_options(tech42 PRIVATE
            -Wno-unknown-warning-option
            -Wno-sign-compare
            -Wno-unused-parameter
            -Wno-unused-const-variable
            -Wno-unused-but-set-parameter
            -Wno-unused-but-set-variable
            -Wno-absolute-value
            -Wno-null-pointer-subtraction
            -Wno-pointer-to-int-cast
        )
    endif()
    if (FIPS_GCC)
        target_compile_options(tech42 PRIVATE
            -Wno-implicit-fallthrough
            -Wno-enum-conversion
            -Wno-format-truncation
            -Wno-type-limits
        )
    endif()
    if (FIPS_MSVC)
        target_compile_options(tech42 PRIVATE
            /wd4244 /wd4267     # conversion from 'xxx' to 'yyy' possible loss of data
            /wd4146             # unary minus perator applied to unsigned type, result still unsigned
            /wd4018             # signed/unsigned mismatch
            /wd4996             # ...deprecated...
            /wd4311             # pointer trunction from 'xxx' to 'yyy'
        )
    endif()
fips_end_app()
