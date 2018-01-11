set (ICON_DIR "${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_DATADIR}/icons/hicolor")
message (STATUS "${ICON_DIR} is the Icon install dir")

install (DIRECTORY
         16x16
         22x22
         24x24
         32x32
         scalable
         DESTINATION "${ICON_DIR}"
         FILES_MATCHING PATTERN "*.png" PATTERN "*.svg")

install(CODE "execute_process (COMMAND gtk-update-icon-cache -t -f ${ICON_DIR})"
        CODE "message (STATUS \"Updating icon cache\")")
