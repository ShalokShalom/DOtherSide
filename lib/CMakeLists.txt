project(DOtherSide)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)

if (WIN32)
  add_definitions(-DWIN32)
endif()

find_package(Qt5Core)
find_package(Qt5Qml)
find_package(Qt5Gui)
find_package(Qt5Quick)
find_package(Qt5Widgets)

set(HEADERS_LIST
  include/DOtherSide/DOtherSideTypes.h
  include/DOtherSide/DOtherSideTypesCpp.h
  include/DOtherSide/DOtherSide.h
  include/DOtherSide/DosQDeclarative.h
  include/DOtherSide/DosQObjectWrapper.h
  include/DOtherSide/DosQAbstractItemModelWrapper.h
  include/DOtherSide/DosQObject.h
  include/DOtherSide/DosQObjectImpl.h
  include/DOtherSide/DosIQObjectImpl.h
  include/DOtherSide/OnSlotExecutedHandler.h
  include/DOtherSide/DosQMetaObject.h
  include/DOtherSide/DosIQAbstractItemModelImpl.h
  include/DOtherSide/DosQAbstractItemModel.h
  include/DOtherSide/Utils.h
  )

set(SRC_LIST
  src/DOtherSide.cpp
  src/OnSlotExecutedHandler.cpp
  src/DosQMetaObject.cpp
  src/DosQDeclarative.cpp
  src/DosQObject.cpp
  src/DOtherSideTypesCpp.cpp
  src/DosQObjectImpl.cpp
  src/DosQAbstractItemModel.cpp
  )

include_directories(include include/Qt)

set(major 0)
set(minor 6)
set(patch 3)

# Shared version for distributing
add_library(${PROJECT_NAME} SHARED ${SRC_LIST} ${HEADERS_LIST})
set_property(TARGET ${PROJECT_NAME} PROPERTY CXX_STANDARD 11)
set_property(TARGET ${PROJECT_NAME} PROPERTY SOVERSION "${major}.${minor}")
set_property(TARGET ${PROJECT_NAME} PROPERTY VERSION "${major}.${minor}.${patch}")
target_link_libraries(${PROJECT_NAME} PRIVATE Qt5::Core Qt5::Gui Qt5::Widgets Qt5::Qml Qt5::Quick)

# Static version for testing
add_library("${PROJECT_NAME}Static" STATIC ${SRC_LIST} ${HEADERS_LIST})
set_property(TARGET "${PROJECT_NAME}Static" PROPERTY CXX_STANDARD 11)
target_link_libraries("${PROJECT_NAME}Static" PRIVATE Qt5::Core Qt5::Gui Qt5::Widgets Qt5::Qml Qt5::Quick)

# Install directive for header files
install(FILES
  include/DOtherSide/DOtherSide.h
  include/DOtherSide/DOtherSideTypes.h
  DESTINATION include/DOtherSide)

# Install directive for binaries
install(TARGETS ${PROJECT_NAME}
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
  )
