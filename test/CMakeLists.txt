
if(NOT(Catch2_FOUND OR TARGET Catch2))
	find_package(Catch2 3 QUIET)

	if(NOT Catch2_FOUND)
		include(FetchContent)

		FetchContent_Declare(
			Catch2
			GIT_REPOSITORY https://github.com/catchorg/Catch2.git
			GIT_TAG v3.8.0)

		FetchContent_MakeAvailable(Catch2)
	endif()
endif()

add_executable(mcfp-unit-test ${CMAKE_CURRENT_SOURCE_DIR}/unit-test.cpp)

target_link_libraries(mcfp-unit-test mcfp::mcfp Catch2::Catch2)

if(MSVC)
	# Specify unwind semantics so that MSVC knowns how to handle exceptions
	target_compile_options(mcfp-unit-test PRIVATE /EHsc)
endif()

add_test(NAME mcfp-unit-test
	COMMAND $<TARGET_FILE:mcfp-unit-test> --data-dir ${CMAKE_CURRENT_SOURCE_DIR})
