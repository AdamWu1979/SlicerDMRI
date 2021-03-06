
#-----------------------------------------------------------------------------
set(BASELINE ${CMAKE_CURRENT_SOURCE_DIR}/../Data/Baseline)
set(DATADIR ${CMAKE_CURRENT_SOURCE_DIR}/../../DWIToDTIEstimation/Data)
set(CLP ${MODULE_NAME})

#-----------------------------------------------------------------------------
add_executable(${CLP}Test ${CLP}Test.cxx)
target_link_libraries(${CLP}Test ${CLP}Lib ${SlicerExecutionModel_EXTRA_EXECUTABLE_TARGET_LIBRARIES})
set_target_properties(${CLP}Test PROPERTIES LABELS ${CLP})
set_target_properties(${CLP}Test PROPERTIES FOLDER ${${CLP}_TARGETS_FOLDER})

set(testname ${CLP}Test)
add_test(NAME ${testname} COMMAND ${SEM_LAUNCH_COMMAND} $<TARGET_FILE:${CLP}Test>
  --compare ${BASELINE}/${CLP}Test_EstimatedBaseline.nhdr ${TEMP}/${CLP}Test_EstimatedBaseline.nhdr
  --compare ${BASELINE}/${CLP}Test_OtsuThresholdMask.nhdr ${TEMP}/${CLP}Test_OtsuThresholdMask.nhdr
  ${CLP}Test
    ${DATADIR}/helix-DWI.nhdr
    ${TEMP}/${CLP}Test_EstimatedBaseline.nhdr
    ${TEMP}/${CLP}Test_OtsuThresholdMask.nhdr
  )
set_property(TEST ${testname} PROPERTY LABELS ${CLP})
