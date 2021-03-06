CMAKE_MINIMUM_REQUIRED( VERSION 3.0 )

PROJECT( "RapaDirectShowViewer" )

IF( MSVC )
	INCLUDE( RapaConfigureVisualStudio )
ENDIF()

SET(	SOURCES
		RDShowQImageWidget.h
		RDShowQImageWidget.cpp
		RDShowQDeviceWidget.h
		RDShowQDeviceWidget.cpp
		RDShowQDeviceManagerWidget.h
		RDShowQDeviceManagerWidget.cpp
		Main.cpp 
	)
	
SOURCE_GROUP("" FILES ${SOURCES} )		# Avoid "Header Files" and "Source Files" virtual folders in VisualStudio
	
INCLUDE( RapaFindQt )
IF( RAPA_QT_FOUND )
	
	INCLUDE_DIRECTORIES( ${RapaDirectShow_SOURCE_DIR} )
	ADD_EXECUTABLE( ${PROJECT_NAME} WIN32 ${SOURCES} )
	TARGET_LINK_LIBRARIES( ${PROJECT_NAME} RapaDirectShow )
	TARGET_LINK_LIBRARIES( ${PROJECT_NAME} ${RAPA_QT_LINK_LIBRARIES}) 

	#
	# Install
	#
	IF( CMAKE_SYSTEM_NAME MATCHES "Windows" )
		INSTALL( TARGETS  ${PROJECT_NAME}
				 CONFIGURATIONS Debug
				 RUNTIME DESTINATION "bin/debug" 
				 LIBRARY DESTINATION "lib"
				 ARCHIVE DESTINATION "lib" )
		INSTALL( TARGETS  ${PROJECT_NAME}
				 CONFIGURATIONS Release
				 RUNTIME DESTINATION "bin/release" 
				 LIBRARY DESTINATION "lib"
				 ARCHIVE DESTINATION "lib" )
		INSTALL( FILES ${RAPA_QT_DEBUG_DLLS} CONFIGURATIONS Debug DESTINATION "bin/debug")
		INSTALL( FILES ${RAPA_QT_DEBUG_PLATFORMS_DLLS} CONFIGURATIONS Debug DESTINATION "bin/debug/platforms")
		INSTALL( FILES ${RAPA_QT_RELEASE_DLLS} CONFIGURATIONS Release DESTINATION "bin/release")
		INSTALL( FILES ${RAPA_QT_RELEASE_PLATFORMS_DLLS} CONFIGURATIONS Release DESTINATION "bin/release/platforms")
	ELSE()
		INSTALL( TARGETS  ${PROJECT_NAME}
				 RUNTIME DESTINATION "bin" 
				 LIBRARY DESTINATION "lib"
				 ARCHIVE DESTINATION "lib" )
	ENDIF()

ELSE()
	MESSAGE("QT not found")
ENDIF()
