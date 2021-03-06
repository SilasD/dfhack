# don't use this file directly. use the one in the root folder of the project

# this is required to ensure we use the right configuration for the system.
IF(UNIX)
add_definitions(-DLINUX_BUILD)
ENDIF(UNIX)

# buildingsdump - dump buildings and their raw data filtered by type
ADD_EXECUTABLE(dfbuildingsdump buildingsdump.cpp)
TARGET_LINK_LIBRARIES(dfbuildingsdump dfhack)

# constructiondump - dump constructions!
ADD_EXECUTABLE(dfconstructiondump construction_dump.cpp)
TARGET_LINK_LIBRARIES(dfconstructiondump dfhack)

# creaturedump - basic creature dump - a test of the creature related exports
ADD_EXECUTABLE(dfcreaturedump creaturedump.cpp)
TARGET_LINK_LIBRARIES(dfcreaturedump dfhack)

# materialtest - just list the first material of each type
ADD_EXECUTABLE(dfmaterialtest materialtest.cpp)
TARGET_LINK_LIBRARIES(dfmaterialtest dfhack)

# position - check the DF window and cursor parameters
ADD_EXECUTABLE(dfposition position.cpp)
TARGET_LINK_LIBRARIES(dfposition dfhack)

# itemdump - dump the item under the cursor
ADD_EXECUTABLE(dfitemdump dfitemdump.cpp)
TARGET_LINK_LIBRARIES(dfitemdump dfhack)

# hotkeynotedump - dumps the hotkeys and notes for the loaded map
# Author: belal
ADD_EXECUTABLE(dfhotkeynotedump hotkeynotedump.cpp)
TARGET_LINK_LIBRARIES(dfhotkeynotedump dfhack)

# settlementdump - dumps the settlements on the loaded map
# Author: belal
# ADD_EXECUTABLE(dfsettlementdump settlementdump.cpp)
# TARGET_LINK_LIBRARIES(dfsettlementdump dfhack)

# treedump - dump them trees!
ADD_EXECUTABLE(dftreedump treedump.cpp)
TARGET_LINK_LIBRARIES(dftreedump dfhack)

# spatterdump - dump spatter 'veins'
ADD_EXECUTABLE(dfspatterdump spatterdump.cpp)
TARGET_LINK_LIBRARIES(dfspatterdump dfhack)

# processenum - demonstrates the use of ProcessEnumerator
ADD_EXECUTABLE(dfprocessenum processenum.cpp)
TARGET_LINK_LIBRARIES(dfprocessenum dfhack)

IF(UNIX)
    SET(CURSES_NEED_WIDE "YES")
    SET(CURSES_NEED_NCURSES "NO")
    find_package(Curses QUIET)
    
    IF(CURSES_FOUND)
        if(CURSES_HAVE_NCURSESW_NCURSES_H)
            SET(NCURSES_H "ncursesw/ncurses.h")
        elseif(CURSES_HAVE_NCURSESW_CURSES_H)
            SET(NCURSES_H "ncursesw/curses.h")
        elseif(CURSES_HAVE_NCURSESW_H)
            SET(NCURSES_H "ncursesw.h")
        elseif(CURSES_HAVE_CURSESW_H)
            SET(NCURSES_H "cursesw.h")
        endif(CURSES_HAVE_NCURSESW_NCURSES_H)
        IF(NCURSES_H)
            # OPTION( VARIABLE "Description" Initial state)
            #OPTION( WITH_FOO "Enable FOO support" ON )
            #OPTION( WITH_BAR "Enable BAR component" OFF )
            #SET( BAZ 18 )
            CONFIGURE_FILE( ${CMAKE_CURRENT_SOURCE_DIR}/fake-curses.h.cmake ${CMAKE_CURRENT_SOURCE_DIR}/fake-curses.h )
            
            # veinlook - look at the map... sort of
            ADD_EXECUTABLE(dfveinlook veinlook.cpp)
            INCLUDE_DIRECTORIES(${CURSES_INCLUDE_DIR})
            TARGET_LINK_LIBRARIES(dfveinlook dfhack ${CURSES_LIBRARIES})
            install(TARGETS
            dfveinlook
            RUNTIME DESTINATION bin
            )
        ENDIF(NCURSES_H)
    ELSE(CURSES_FOUND)
        MESSAGE(STATUS "Wide-character ncurses library not found - veinlook can't be built")
    ENDIF(CURSES_FOUND)
ENDIF(UNIX)