
#-----------------------------------------------------------------------------
set(BASELINE ${CMAKE_CURRENT_SOURCE_DIR}/../Data/Baseline)
set(INPUT ${CMAKE_CURRENT_SOURCE_DIR}/../Data/Input)
set(CLP ${MODULE_NAME})

#-----------------------------------------------------------------------------
add_executable(${CLP}Test ${CLP}Test.cxx)
target_link_libraries(${CLP}Test ${CLP}Lib ${SlicerExecutionModel_EXTRA_EXECUTABLE_TARGET_LIBRARIES})
set_target_properties(${CLP}Test PROPERTIES LABELS ${CLP})
set_target_properties(${CLP}Test PROPERTIES FOLDER ${${CLP}_TARGETS_FOLDER})

set(testname ${CLP}Test)
add_test(NAME ${testname} COMMAND ${SEM_LAUNCH_COMMAND} $<TARGET_FILE:${CLP}Test>
  --compare ${BASELINE}/OAS10001-255.mha
            ${TEMP}/${CLP}TestOutput.mha
  --compareNumberOfPixelsTolerance 20
  ModuleEntryPoint
    ${INPUT}/OAS10001.hdr
    ${INPUT}/OAS10001-Transformed.vtp
    ${TEMP}/${CLP}TestOutput.mha
  )
set_property(TEST ${testname} PROPERTY LABELS ${CLP})

# With a different output label
set(testname ${CLP}TestLabelValue)
add_test(NAME ${testname} COMMAND ${SEM_LAUNCH_COMMAND} $<TARGET_FILE:${CLP}Test>
  --compare ${BASELINE}/OAS10001-128.mha
            ${TEMP}/${CLP}TestLabelValueOutput.mha
  --compareNumberOfPixelsTolerance 20
  ModuleEntryPoint
    --labelValue 128
    ${INPUT}/OAS10001.hdr
    ${INPUT}/OAS10001-Transformed.vtp
    ${TEMP}/${CLP}TestLabelValueOutput.mha
  )
set_property(TEST ${testname} PROPERTY LABELS ${CLP})
