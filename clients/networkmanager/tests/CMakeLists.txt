find_package(GTest REQUIRED)
find_package(PkgConfig REQUIRED)
pkg_check_modules(Systemd REQUIRED IMPORTED_TARGET libsystemd)
find_program(DBUS_RUN_SESSION dbus-run-session REQUIRED)

add_executable(properdesktop-networkmanager-client-tests
    ip_utils_test.cpp
    vpn_schema_test.cpp
    vpn_utils_test.cpp
    wifi_utils_test.cpp
)
set_target_properties(properdesktop-networkmanager-client-tests PROPERTIES AUTOMOC OFF AUTORCC OFF)
target_link_libraries(properdesktop-networkmanager-client-tests PRIVATE properdesktop-networkmanager-client GTest::gtest_main)
include(GoogleTest)
gtest_discover_tests(properdesktop-networkmanager-client-tests)

add_executable(properdesktop-networkmanager-client-bus-tests client_test.cpp fake_network_manager.h)
set_target_properties(properdesktop-networkmanager-client-bus-tests PROPERTIES AUTOMOC OFF AUTORCC OFF)
target_link_libraries(properdesktop-networkmanager-client-bus-tests PRIVATE properdesktop-networkmanager-client PkgConfig::Systemd GTest::gtest)
# The client and the fake NetworkManager talk over a private session bus.
add_test(NAME properdesktop-networkmanager-client-bus-tests
         COMMAND ${DBUS_RUN_SESSION} -- $<TARGET_FILE:properdesktop-networkmanager-client-bus-tests>)

add_executable(properdesktop-networkmanager-secretreader-tests secret_reader_test.cpp)
set_target_properties(properdesktop-networkmanager-secretreader-tests PROPERTIES AUTOMOC OFF AUTORCC OFF)
target_link_libraries(properdesktop-networkmanager-secretreader-tests PRIVATE properdesktop-networkmanager-client GTest::gtest)
# A fake Secret Service on a private session bus.
add_test(NAME properdesktop-networkmanager-secretreader-tests
         COMMAND ${DBUS_RUN_SESSION} -- $<TARGET_FILE:properdesktop-networkmanager-secretreader-tests>)
