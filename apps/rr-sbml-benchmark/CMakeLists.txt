CMAKE_MINIMUM_REQUIRED(VERSION 2.6.3 FATAL_ERROR)
PROJECT(RR_CXX_API_TESTS)

set(target rr-sbml-benchmark)

set(tests
    tests/base
    tests/sbml_test_suite
    tests/steady_state
    tests/stoichiometric
    )

add_executable( ${target}
    main
    )

set_property(TARGET ${target}
    PROPERTY  COMPILE_DEFINITIONS
    LIBSBML_USE_CPP_NAMESPACE
    LIBSBML_STATIC
    STATIC_LIBSTRUCT
    STATIC_PUGI
    STATIC_RR
    STATIC_NLEQ
    )

link_directories(
    ${SBMLSOLVER_DEP_DIR}/lib
    )

include_directories(
    src
    ${RR_ROOT}
    ${SBMLSOLVER_DEP_DIR}/include/clapack
    )

if(UNIX)
    set(staticLibPrefix ".a")
    set(sharedLibPrefix ".so")
else()
    set(staticLibPrefix "")
    set(sharedLibPrefix "")
endif()

if(WIN32)
    target_link_libraries (${target}
        sbmlsolver_static
        unit_test-static${staticLibPrefix}
        )
endif()

if(UNIX)
    target_link_libraries (${target}
        sbmlsolver_static
        libunit_test-static${staticLibPrefix}
        lapack
        blas
        f2c
        dl
        )
endif()


install (TARGETS ${target}
    DESTINATION bin
    COMPONENT testing
    )


