project(DCHdigi)

file(GLOB sources
    ${PROJECT_SOURCE_DIR}/src/*.cpp
)

file(GLOB headers
  ${PROJECT_SOURCE_DIR}/include/*.h
)

gaudi_add_module(DCHdigi
  SOURCES ${sources}
  LINK
  Gaudi::GaudiKernel
  EDM4HEP::edm4hep
  k4FWCore::k4FWCore
)

target_include_directories(DCHdigi PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

set_target_properties(DCHdigi PROPERTIES PUBLIC_HEADER "${headers}")

file(GLOB scripts
  ${PROJECT_SOURCE_DIR}/test/*.py
)

file(COPY ${scripts} DESTINATION ${CMAKE_CURRENT_BINARY_DIR}/test)

install(TARGETS DCHdigi
  EXPORT ${CMAKE_PROJECT_NAME}Targets
  RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}" COMPONENT bin
  LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}" COMPONENT shlib
  PUBLIC_HEADER DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/@{CMAKE_PROJECT_NAME}" COMPONENT dev
)

install(FILES ${scripts} DESTINATION test)

#############################################
#file(GLOB _plugin_sources src/components/*.cpp)
#gaudi_add_module(k4ProjectTemplatePlugins
#                 SOURCES ${_plugin_sources}
#                 LINK Gaudi::GaudiKernel 
#                      Gaudi::GaudiAlgLib
#                      k4FWCore::k4FWCore
#                      EDM4HEP::edm4hep
#                      )
#
#install(TARGETS k4ProjectTemplatePlugins
#  EXPORT ${CMAKE_PROJECT_NAME}Targets
#  RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}" COMPONENT bin
#  LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}" COMPONENT shlib
#  PUBLIC_HEADER DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}/@{CMAKE_PROJECT_NAME}"
#  COMPONENT dev)
#
#include(CTest)
#
##--- The genConf directory has been renamed to genConfDir in Gaudi 35r1
##--- See https://gitlab.cern.ch/gaudi/Gaudi/-/merge_requests/1158
#set(GAUDI_GENCONF_DIR "genConfDir")
#if (${Gaudi_VERSION} VERSION_LESS 35.1)
#  set(GAUDI_GENCONF_DIR "genConf")
#endif()
#
#function(set_test_env _testname)
#  set_property(TEST ${_testname} APPEND PROPERTY ENVIRONMENT
#    LD_LIBRARY_PATH=${CMAKE_BINARY_DIR}:$<TARGET_FILE_DIR:k4ProjectTemplatePlugins>:$<TARGET_FILE_DIR:ROOT::Core>:$<TARGET_FILE_DIR:k4FWCore::k4FWCore>:$<TARGET_FILE_DIR:EDM4HEP::edm4hep>:$<TARGET_FILE_DIR:podio::podio>:$ENV{LD_LIBRARY_PATH}
#    PYTHONPATH=${CMAKE_BINARY_DIR}/${CMAKE_PROJECT_NAME}/${GAUDI_GENCONF_DIR}:$<TARGET_FILE_DIR:k4FWCore::k4FWCore>/../python:$ENV{PYTHONPATH}
#    PATH=$<TARGET_FILE_DIR:k4FWCore::k4FWCore>/../bin:$ENV{PATH}
#    K4PROJECTTEMPLATE=${CMAKE_CURRENT_LIST_DIR}/
#    )
#endfunction()
#
#add_test(NAME CreateExampleEventData
#               WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}
#               COMMAND k4run k4ProjectTemplate/options/createExampleEventData.py)
#set_test_env(CreateExampleEventData)
#
#add_test(NAME ReadExampleEventData
#               WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}
#               COMMAND k4run k4ProjectTemplate/options/readExampleEventData.py)
#set_test_env(ReadExampleEventData)
#set_property(TEST ReadExampleEventData APPEND PROPERTY DEPENDS CreateExampleEventData)
#
#add_test(NAME HelloWorldAlg
#               WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}
#               COMMAND k4run k4ProjectTemplate/options/createHelloWorld.py)
#set_test_env(HelloWorldAlg)
