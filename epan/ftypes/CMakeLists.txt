# CMakeLists.txt
#
# Wireshark - Network traffic analyzer
# By Gerald Combs <gerald@wireshark.org>
# Copyright 1998 Gerald Combs
#
# SPDX-License-Identifier: GPL-2.0-or-later
#

include_directories(
	${CMAKE_CURRENT_BINARY_DIR}
	${CMAKE_CURRENT_SOURCE_DIR}
	${CMAKE_SOURCE_DIR}/epan
)

set(FTYPE_PUBLIC_HEADERS
	ftypes.h
)

set(FTYPE_FILES
	ftypes.c
	ftype-bytes.c
	ftype-double.c
	ftype-ieee-11073-float.c
	ftype-integer.c
	ftype-ipv4.c
	ftype-ipv6.c
	ftype-guid.c
	ftype-none.c
	ftype-pcre.c
	ftype-protocol.c
	ftype-string.c
	ftype-time.c
)
source_group(ftype FILES ${FTYPE_FILES})

set_source_files_properties(
	${FTYPE_FILES}
	PROPERTIES
	COMPILE_FLAGS "${WERROR_COMMON_FLAGS}"
)

add_library(ftypes OBJECT
	${FTYPE_FILES}
)

set_target_properties(ftypes PROPERTIES
	FOLDER "Libs/epan/ftypes"
	COMPILE_DEFINITIONS "WS_BUILD_DLL"
)

if(NOT WIN32)
	install(FILES ${FTYPE_PUBLIC_HEADERS}
		DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/${CPACK_PACKAGE_NAME}/epan/ftypes"
	)
endif()

CHECKAPI(
	NAME
	  ftypes
	SWITCHES
	  -g termoutput -build
	SOURCES
	  ${FTYPE_FILES}
)

#
# Editor modelines  -  http://www.wireshark.org/tools/modelines.html
#
# Local variables:
# c-basic-offset: 8
# tab-width: 8
# indent-tabs-mode: t
# End:
#
# vi: set shiftwidth=8 tabstop=8 noexpandtab:
# :indentSize=8:tabSize=8:noTabs=false:
#
