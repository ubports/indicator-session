include (Translations)
add_translations_directory ("${GETTEXT_PACKAGE}")
add_translations_catalog ("${GETTEXT_PACKAGE}" ../src/ ../src/dbus-backend)

find_package(Intltool REQUIRED)

intltool_update_potfile(
    ALL
    KEYWORDS "_" "_:1,2" "N_" "N_:1,2"
    GETTEXT_PACKAGE ${GETTEXT_PACKAGE}
    COPYRIGHT_HOLDER "Canonical Ltd."
)

