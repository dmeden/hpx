# Copyright (c) 2016 Hartmut Kaiser
#
# Distributed under the Boost Software License, Version 1.0. (See accompanying
# file LICENSE_1_0.txt or copy at http://www.boost.org/LICENSE_1_0.txt)

set(tests)

if(HPX_WITH_DATAPAR_VC OR HPX_WITH_DATAPAR_BOOST_SIMD OR HPX_WITH_DATAPAR_LIBFLATARRAY)
  set(tests
      count_datapar
      countif_datapar
      foreach_datapar
      foreach_datapar_zipiter
      foreachn_datapar
      transform_datapar
      transform_binary_datapar
      transform_binary2_datapar
      transform_reduce_binary_datapar
     )
endif()

foreach(test ${tests})
  set(sources
      ${test}.cpp)

  set(${test}_PARAMETERS THREADS_PER_LOCALITY 4)

  source_group("Source Files" FILES ${sources})

  # add example executable
  add_hpx_executable(${test}_test
                     SOURCES ${sources}
                     ${${test}_FLAGS}
                     EXCLUDE_FROM_ALL
                     HPX_PREFIX ${HPX_BUILD_PREFIX}
                     FOLDER "Tests/Unit/Parallel/DataParAlgorithms")

  add_hpx_unit_test("parallel" ${test} ${${test}_PARAMETERS})

  # add a custom target for this example
  add_hpx_pseudo_target(tests.unit.parallel.datapar_algorithms.${test})

  # make pseudo-targets depend on master pseudo-target
  add_hpx_pseudo_dependencies(tests.unit.parallel.datapar_algorithms
                              tests.unit.parallel.datapar_algorithms.${test})

  # add dependencies to pseudo-target
  add_hpx_pseudo_dependencies(tests.unit.parallel.datapar_algorithms.${test}
                              ${test}_test_exe)

endforeach()
