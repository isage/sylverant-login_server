project(sylverant-login-l10n)

set(LANGS de fr sp)

if (UNIX AND NOT APPLE)
    foreach(LANG ${LANGS})
        install(FILES "sylverant-login-${LANG}.yts" DESTINATION "share/sylverant/l10n")
    endforeach()
endif ()

