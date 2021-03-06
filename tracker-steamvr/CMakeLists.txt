set(steamvr-dir "")
set(steamvr-dll "")
set(steamvr-lib "")

if(LINUX AND opentrack-64bit)
    set(steamvr-dir "linux64")
    set(steamvr-dll "libopenvr_api.so")
    set(steamvr-lib "${steamvr-dll}")
endif()

if(WIN32)
    if(opentrack-64bit)
        set(steamvr-dir "win64")
    else()
        set(steamvr-dir "win32")
    endif()
    set(steamvr-dll "openvr_api.dll")
    set(steamvr-lib "openvr_api.lib")
endif()

if(APPLE AND NOT opentrack-64bit)
    set(steamvr-dir "osx32")
    set(steamvr-dll "libopenvr_api.dylib")
    set(steamvr-lib "${steamvr-dll}")
endif()

if(steamvr-dll)
    SET(SDK_VALVE_STEAMVR "" CACHE PATH "Valve's SteamVR")
    if(SDK_VALVE_STEAMVR)
        opentrack_boilerplate(opentrack-tracker-steamvr)
        install(FILES "${SDK_VALVE_STEAMVR}/bin/${steamvr-dir}/${steamvr-dll}" DESTINATION "${opentrack-hier-pfx}")

        target_include_directories(opentrack-tracker-steamvr SYSTEM PUBLIC "${SDK_VALVE_STEAMVR}/headers")
        target_link_libraries(opentrack-tracker-steamvr "${SDK_VALVE_STEAMVR}/lib/${steamvr-dir}/${steamvr-lib}")
    endif()
endif()
