#=============================================================================
#  Mscore
#  Linux Music Score Editor
#  $Id:$
#
#  Copyright (C) 2007 by Werner Schweer and others
#
#  This program is free software; you can redistribute it and/or modify
#  it under the terms of the GNU General Public License version 2.
#
#  This program is distributed in the hope that it will be useful,
#  but WITHOUT ANY WARRANTY; without even the implied warranty of
#  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#  GNU General Public License for more details.
#
#  You should have received a copy of the GNU General Public License
#  along with this program; if not, write to the Free Software
#  Foundation, Inc., 675 Mass Ave, Cambridge, MA 02139, USA.
#=============================================================================

include (${PROJECT_SOURCE_DIR}/build/gch.cmake)

if (APPLE)
      file(GLOB_RECURSE INCS "*.h")
else (APPLE)
        set(INCS "")
endif (APPLE)

if (SOUNDFONT3)
      set(SF3_SRC sfont3.cpp)
endif (SOUNDFONT3)

QT5_WRAP_UI (fluidUi fluid_gui.ui)

if (NOT MSVC)
   set(_all_h_file "${PROJECT_BINARY_DIR}/all.h")
else (NOT MSVC)
   set(_all_h_file "${PROJECT_SOURCE_DIR}/all.h")
endif (NOT MSVC)

add_library (fluid STATIC
      ${_all_h_file}
      ${PCH}
      ${fluidUi}
      fluidgui.cpp
      dsp.cpp fluid.cpp voice.cpp chan.cpp sfont.cpp
      conv.cpp gen.cpp mod.cpp
      conv.h fluid.h fluidgui.h gen.h sfont.h voice.h
      ${SF3_SRC}
      ${INCS}
      )
if (NOT MSVC)
   set_target_properties (
      fluid
      PROPERTIES
         COMPILE_FLAGS "${PCH_INCLUDE} -g -Wall -Wextra -Winvalid-pch"
      )
else (NOT MSVC)
   set_target_properties (
      fluid
      PROPERTIES
         COMPILE_FLAGS "${PCH_INCLUDE}"
      )
endif (NOT MSVC)   

xcode_pch(fluid all)

# Use MSVC pre-compiled headers
vstudio_pch( fluid )

# MSVC does not depend on mops1 & mops2 for PCH
if (NOT MSVC)
   ADD_DEPENDENCIES(fluid mops1)
   ADD_DEPENDENCIES(fluid mops2)
endif (NOT MSVC)   

