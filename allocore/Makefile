#=========================================================================
# AlloCore main makefile
#=========================================================================

# We used to include another configuration file here, but it doesn't seem
# to serve any purpose other than add complexity.
#include Makefile.config
# BEGIN old Makefile.config
LIB_NAME	= allocore
include ../Makefile.common


BIG_ENDIAN	= 0
AUDIO_BINDING	= PortAudio
WINDOW_BINDING	= GLUT
RUNTIME_BINDING	= APR
IMAGE_BINDING	= FreeImage
AUTORUN_TESTS	= 1
STRICT_WARNINGS = 1

LIB_DIRS	= /usr/lib/
INC_DIRS	= /usr/include/ 

#-------------------------------------------------------------------------
# Default variables
#-------------------------------------------------------------------------
#INC_DIR		= $(LIB_NAME)/
INC_DIR		= ./
SRC_DIR		= src/
GFX_DIR		= graphics/
IO_DIR		= io/
MATH_DIR	= math/
PRO_DIR		= protocol/
SPA_DIR		= spatial/
SND_DIR		= sound/
SYS_DIR		= system/
TYP_DIR		= types/
TEST_DIR	= unitTests/

# These are defined below according to platform
DEV_DIR		=
DEV_LIB_DIR 	=

# Source file directories for running executables
RUN_INC_DIRS	?=
RUN_SRC_DIRS	?=
RUN_DIRS	?=
RUN_DIRS	+= examples/

#-------------------------------------------------------------------------
# Platform specific section
#-------------------------------------------------------------------------
# Linux:
ifeq ($(PLATFORM), linux)
	INC_DIRS += $(PREFIX)include/
	LIB_DIRS += $(PREFIX)lib/
#	DEV_DIR   = linux/
#	DEV_LIB_DIR = linux/lib$(ARCH)

# Mac OS X:
else ifeq ($(PLATFORM), macosx)
	LDFLAGS	+= -framework Carbon -framework Cocoa -framework IOKit
#	DEV_DIR  = osx/
#	DEV_LIB_DIR = osx/lib

# Windows
else ifeq ($(PLATFORM), windows)
endif

#-------------------------------------------------------------------------
# Finalize compilation flags
#-------------------------------------------------------------------------
INC_DIRS += $(INC_DIR) 

#INC_DIRS += $(DEV_DIR)include/
#LIB_DIRS += $(DEV_DIR)lib/

ifeq ($(RUNTIME_BINDING), APR)
	LINK_LDFLAGS += -lapr-1 -laprutil-1
endif

VPATH += $(addprefix $(SRC_DIR), $(GFX_DIR) $(IO_DIR) $(PRO_DIR) $(MATH_DIR) $(SND_DIR) $(SPA_DIR) $(SYS_DIR) $(TYP_DIR))

# END old Makefile.config



# Include configuration files of AlloCore modules
# TODO: Permit selective inclusive of modules for building a library
# and doing unit tests.

MODULE_DIRS := $(GFX_DIR) $(IO_DIR) $(MATH_DIR) $(PRO_DIR) $(SND_DIR) $(SPA_DIR) $(SYS_DIR) $(TYP_DIR)

include $(SRC_DIR)$(GFX_DIR)Makefile.config
include $(SRC_DIR)$(IO_DIR)Makefile.config
include $(SRC_DIR)$(MATH_DIR)Makefile.config
include $(SRC_DIR)$(PRO_DIR)Makefile.config
include $(SRC_DIR)$(SND_DIR)Makefile.config
include $(SRC_DIR)$(SPA_DIR)Makefile.config
include $(SRC_DIR)$(SYS_DIR)Makefile.config
include $(SRC_DIR)$(TYP_DIR)Makefile.config

# Prefix full path to source files
GFX_SRC		:= $(addprefix $(SRC_DIR)$(GFX_DIR), $(GFX_SRC))
IO_SRC		:= $(addprefix $(SRC_DIR)$(IO_DIR), $(IO_SRC))
MATH_SRC	:= $(addprefix $(SRC_DIR)$(MATH_DIR), $(MATH_SRC))
PRO_SRC		:= $(addprefix $(SRC_DIR)$(PRO_DIR), $(PRO_SRC))
TYP_SRC		:= $(addprefix $(SRC_DIR)$(TYP_DIR), $(TYP_SRC))
SND_SRC		:= $(addprefix $(SRC_DIR)$(SND_DIR), $(SND_SRC))
SPA_SRC		:= $(addprefix $(SRC_DIR)$(SPA_DIR), $(SPA_SRC))
SYS_SRC		:= $(addprefix $(SRC_DIR)$(SYS_DIR), $(SYS_SRC))

# These are all the source files
SRCS		= \
		$(GFX_SRC) $(IO_SRC) $(PRO_SRC) $(MATH_SRC) \
		$(SND_SRC) $(SPA_SRC) $(SYS_SRC) $(TYP_SRC)

OBJS		= $(addsuffix .o, $(basename $(notdir $(SRCS))))

CPPFLAGS	+= $(addprefix -I, $(INC_DIRS) $(BUILD_DIR)/include)
LDFLAGS		:= $(addprefix -L, $(LIB_DIRS) $(BUILD_DIR)/lib) $(LDFLAGS)



#VPATH += allocore/graphics/ allocore/io/ allocore/math/
#PCHS		=
#PCHS		+= $(addsuffix .gch, $(notdir $(wildcard allocore/graphics/*.hpp)))
#PCHS		+= $(addsuffix .gch, $(notdir $(wildcard allocore/io/*.hpp)))
#PCHS		+= $(addsuffix .gch, $(notdir $(wildcard allocore/math/*.hpp)))

#--------------------------------------------------------------------------
# Rules
#--------------------------------------------------------------------------

include ../Makefile.rules
include ../Makefile.buildandrun

# Build library
#allocore: $(LIB_PATH)
## 	Copy library header files to build directory
#	@for v in `cd $(INC_DIR)/$@ && find * -type d ! -path '*.*'` .; do\
#		$(INSTALL) -d $(BUILD_DIR)/include/$@/$$v;\
#		$(INSTALL) -C -m 644 $(INC_DIR)/$@/$$v/*.h* $(BUILD_DIR)/include/$@/$$v;\
#	done

##	Copy dependency headers to build folder
#	@(cd $(DEV_DIR)/include && tar -cf - `find . -type f ! -name ".*" ! -path "*.svn*" -print` )\
#		| ( cd $(BUILD_DIR)/include && tar xBf - )

## 	Copy dependency binaries to build folder
##	Copying only occurs if the destination file doesn't exist or the source file is newer
#	@for v in `cd $(DEV_LIB_DIR) && find . -name \*.$(SLIB_EXT) -or -name \*.$(DLIB_EXT)`; do\
#		if [ $(DEV_LIB_DIR)/$$v -nt $(BUILD_DIR)/lib/$$v ] || [ ! -e $(BUILD_DIR)/lib/$$v ]; then\
#			echo Copying $(DEV_LIB_DIR)/$$v to $(BUILD_DIR)/lib/;\
#			$(INSTALL) -C -m 644 $(DEV_LIB_DIR)/$$v $(BUILD_DIR)/lib/;\
#		fi;\
#	done


# Install library into path specified by DESTDIR
# Include files are copied into DESTDIR/include/LIB_NAME
# Library files are copied into DESTDIR/lib
install: $(LIB_PATH)

## 	Copy library header files to build directory
	@for v in `cd $(INC_DIR)/$(LIB_NAME) && find * -type d ! -path '*.*'` .; do\
		$(INSTALL) -d $(DESTDIR)/include/$(LIB_NAME)/$$v;\
		$(INSTALL) -m 644 $(INC_DIR)/$(LIB_NAME)/$$v/*.h* $(DESTDIR)/include/$(LIB_NAME)/$$v;\
	done

##	Copy library if it doesn't exist or is newer than existing
	@$(INSTALL) -d $(DESTDIR)/lib/
	@if [ $(LIB_PATH) -nt $(DESTDIR)/lib/$(LIB_FILE) ] || [ ! -e $(DESTDIR)/lib/$(LIB_FILE) ]; then \
		echo INSTALL $(LIB_PATH) to $(DESTDIR)/lib/;\
		$(INSTALL) -m 644 $(LIB_PATH) $(DESTDIR)/lib/;\
	fi;

##	Install header files from local build directory into DESTDIR
#	@for v in `cd $(BUILD_DIR)/include && find * -type d ! -path '*.*'`; do \
#		$(INSTALL) -d $(DESTDIR)/include/$$v; \
#		$(INSTALL) -C -m 644 $(BUILD_DIR)/include/$$v/*.h* $(DESTDIR)/include/$$v;\
#	done

## 	Install library files from local build directory into DESTDIR
#	@for v in `cd $(BUILD_DIR)/lib && find * -type d ! -path '*.*'` .; do \
#		$(INSTALL) -d $(DESTDIR)/lib/$$v; \
#		$(INSTALL) -C -m 644 $(BUILD_DIR)/lib/$$v/*.a $(DESTDIR)/lib/$$v; \
#	done


#
buildtest: test
	@for v in graphics io simulation sound spatial system; do \
		$(MAKE) --no-print-directory examples/$$v/*.cpp AUTORUN=0; \
	done

# Build unit tests
test: linkfile $(LIB_PATH)
	@$(MAKE) -C $(TEST_DIR) test


# Remove build files
.PHONY: clean
clean:
	@$(RM) $(LIB_PATH)
	$(call RemoveDir, $(OBJ_DIR))
	@$(MAKE) -C $(TEST_DIR) clean

