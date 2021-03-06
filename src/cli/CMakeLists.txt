#  Copyright (C) 2017 KeePassXC Team
#
#  This program is free software: you can redistribute it and/or modify
#  it under the terms of the GNU General Public License as published by
#  the Free Software Foundation, either version 2 or (at your option)
#  version 3 of the License.
#
#  This program is distributed in the hope that it will be useful,
#  but WITHOUT ANY WARRANTY; without even the implied warranty of
#  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#  GNU General Public License for more details.
#
#  You should have received a copy of the GNU General Public License
#  along with this program.  If not, see <http://www.gnu.org/licenses/>.

set(cli_SOURCES
        Add.cpp
        Clip.cpp
        Create.cpp
        Command.cpp
        Diceware.cpp
        Edit.cpp
        Estimate.cpp
        Extract.cpp
        Generate.cpp
        List.cpp
        Locate.cpp
        Merge.cpp
        Remove.cpp
        Show.cpp)

add_library(cli STATIC ${cli_SOURCES})
target_link_libraries(cli Qt5::Core Qt5::Widgets)

add_executable(keepassxc-cli keepassxc-cli.cpp)
target_link_libraries(keepassxc-cli
        cli
        keepassx_core
        Qt5::Core
        ${GCRYPT_LIBRARIES}
        ${ARGON2_LIBRARIES}
        ${GPGERROR_LIBRARIES}
        ${ZLIB_LIBRARIES}
        ${ZXCVBN_LIBRARIES})

install(TARGETS keepassxc-cli
        BUNDLE DESTINATION . COMPONENT Runtime
        RUNTIME DESTINATION ${CLI_INSTALL_DIR} COMPONENT Runtime)

if(APPLE OR UNIX)
    install(FILES keepassxc-cli.1 DESTINATION ${CMAKE_INSTALL_MANDIR}/man1/)
    execute_process(COMMAND mandb -q)
endif()
