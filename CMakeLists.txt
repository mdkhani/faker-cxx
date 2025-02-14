cmake_minimum_required(VERSION 3.23)

set(PROJECT_NAME faker-cxx)
project(${PROJECT_NAME} CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED YES)

option(BUILD_TESTS DEFAULT ON)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wpedantic -Wconversion -Wformat -Werror")

set(LIBRARY_NAME faker-cxx)

set(SOURCES
        src/Person.cpp
        src/Internet.cpp
        src/String.cpp
        src/Lorem.cpp
        src/Datatype.cpp
        src/Number.cpp
        src/Date.cpp
        src/DatatypeTest.cpp
        include/faker-cxx/Helper.h
        src/StringHelper.cpp
        src/Finance.cpp
        src/Company.cpp
        src/Color.cpp
        src/Commerce.cpp
        )

set(UT_SOURCES
        src/NumberTest.cpp
        src/StringTest.cpp
        src/PersonTest.cpp
        src/LoremTest.cpp
        src/InternetTest.cpp
        src/DateTest.cpp
        src/StringHelperTest.cpp
        src/FinanceTest.cpp
        src/CompanyTest.cpp
        src/ColorTest.cpp
        src/CommerceTest.cpp
        )

add_library(${LIBRARY_NAME} ${SOURCES})

target_include_directories(${LIBRARY_NAME}
        PUBLIC
        "include"
        PRIVATE
        "include/faker-cxx"
        )

if (BUILD_TESTS)
    find_package(GTest REQUIRED)

    enable_testing()

    set(target_code_coverage_ALL 1)

    include("cmake/cmake-coverage.cmake")

    add_code_coverage_all_targets()

    add_executable(${LIBRARY_NAME}UT ${SOURCES} ${UT_SOURCES})

    target_link_libraries(${LIBRARY_NAME}UT PRIVATE
            GTest::gtest_main
            )

    target_include_directories(${LIBRARY_NAME}UT
            PRIVATE
            "include/faker-cxx"
            ${GTEST_INCLUDE_DIR}
            )

    add_test(NAME ${LIBRARY_NAME}UT COMMAND ${LIBRARY_NAME}UT WORKING_DIRECTORY ${CMAKE_CURRENT_LIST_DIR})

    target_code_coverage(${LIBRARY_NAME}UT ALL)
endif ()
