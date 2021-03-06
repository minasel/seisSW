#========================================================================
#
#                   S P E C F E M 2 D  Version 7 . 0
#                   --------------------------------
#
#     Main historical authors: Dimitri Komatitsch and Jeroen Tromp
#                        Princeton University, USA
#                and CNRS / University of Marseille, France
#                 (there are currently many more authors!)
# (c) Princeton University and CNRS / University of Marseille, April 2014
#
# This software is a computer program whose purpose is to solve
# the two-dimensional viscoelastic anisotropic or poroelastic wave equation
# using a spectral-element method (SEM).
#
# This software is governed by the CeCILL license under French law and
# abiding by the rules of distribution of free software. You can use,
# modify and/or redistribute the software under the terms of the CeCILL
# license as circulated by CEA, CNRS and Inria at the following URL
# "http://www.cecill.info".
#
# As a counterpart to the access to the source code and rights to copy,
# modify and redistribute granted by the license, users are provided only
# with a limited warranty and the software's author, the holder of the
# economic rights, and the successive licensors have only limited
# liability.
#
# In this respect, the user's attention is drawn to the risks associated
# with loading, using, modifying and/or developing or reproducing the
# software by the user in light of its specific status of free software,
# that may mean that it is complicated to manipulate, and that also
# therefore means that it is reserved for developers and experienced
# professionals having in-depth computer knowledge. Users are therefore
# encouraged to load and test the software's suitability as regards their
# requirements in conditions enabling the security of their systems and/or
# data to be ensured and, more generally, to use and operate it in the
# same conditions as regards security.
#
# The full text of the license is available in file "LICENSE".
#
#========================================================================
#
# Makefile.  Generated from Makefile.in by configure.
#######################################

FC = ifort
FCFLAGS = -g
MPIFC = mpif90
MPILIBS = 
CC = gcc

DEF_FFLAGS = -mod ./obj -I./obj -I. -xHost -fpe0 -ftz -assume buffered_io -assume byterecl -align sequence -vec-report0 -std03 -diag-disable 6477 -implicitnone -gen-interfaces -warn all -O3 -check nobounds -I${SETUP}
CPPFLAGS =  $(COND_MPI_CPPFLAGS)
CFLAGS = -g -O2 $(CPPFLAGS) -I${SETUP}

FC_MODEXT = mod
FC_MODDIR = ./obj

## serial or parallel
#F90 = $(MPIFC) $(FCFLAGS) -DUSE_MPI -DUSE_SCOTCH -I"" $(MPILIBS)
F90 = $(FC) $(FCFLAGS)

USE_BUNDLED_SCOTCH = 0
SCOTCH_DIR = 
SCOTCH_INCLUDEDIR = 
SCOTCH_LIBDIR = 

LIB =

## scotch libraries
#LIB += -L"" -lscotch -lscotcherr
LIB +=

LINK = $(F90)


#######################################
####
#### CUDA
#### with configure: ./configure --with-cuda=cuda5 CUDA_FLAGS=.. CUDA_LIB=.. CUDA_INC=.. MPI_INC=.. ..
####
#######################################

#CUDA = yes
CUDA = no

#CUDA5 = yes
CUDA5 = no

# default cuda libraries
# runtime library -lcudart -lcuda and -lcublas needed

CUDA_FLAGS = @CUDA_FLAGS@ 
CUDA_INC = @CUDA_CPPFLAGS@ -I${SETUP}
CUDA_LINK = @CUDA_LDFLAGS@ @CUDA_LIBS@

#NVCC = nvcc
NVCC = gcc

# GPU architecture

# CUDA architecture / code version
# Fermi: -gencode=arch=compute_10,code=sm_10 not supported
# Tesla (default): -gencode=arch=compute_20,code=sm_20
# Geforce GT 650m: -gencode=arch=compute_30,code=sm_30
# Kepler (cuda5) : -gencode=arch=compute_35,code=sm_35
GENCODE_20 = -gencode=arch=compute_20,code=\"sm_20,compute_20\"
GENCODE_30 = -gencode=arch=compute_30,code=\"sm_30,compute_30\"
GENCODE_35 = -gencode=arch=compute_35,code=\"sm_35,compute_35\"

# CUDA version 5.x
##GENCODE = $(GENCODE_35)
# CUDA version 4.x
#GENCODE = $(GENCODE_20)

# CUDA flags and linking
#NVCC_FLAGS_BASE = $(CUDA_FLAGS) $(CUDA_INC) $(COND_MPI_CPPFLAGS)
##NVCC_FLAGS = $(NVCC_FLAGS_BASE) -DCUDA $(GENCODE) -ftz=true
#NVCC_FLAGS = $(NVCC_FLAGS_BASE) -DCUDA -DUSE_OLDER_CUDA4_GPU $(GENCODE)

##NVCCLINK_BASE = $(NVCC) $(CUDA_INC) $(COND_MPI_CPPFLAGS) -DCUDA
##NVCCLINK = $(NVCCLINK_BASE) -dlink $(GENCODE)
#NVCCLINK = $(NVCCLINK_BASE) -DUSE_OLDER_CUDA4_GPU $(GENCODE)

NVCC_FLAGS = $(COND_MPI_CPPFLAGS)
NVCCLINK = $(NVCC) $(NVCC_FLAGS)


#######################################
####
#### directories
####
#######################################

## compilation directories
# B : build directory
B = .
# E : executables directory
E = $B/bin
# O : objects directory
O = $B/obj
# L : libraries directory
L = $B/lib
# setup file directory
SETUP = $B/setup
# output file directory
OUTPUT = $B/OUTPUT_FILES
# S_TOP : source file root directory
S_TOP = .


#######################################
####
#### targets
####
#######################################

# code subdirectories
SUBDIRS = \
	meshfem2D \
	shared \
	cuda \
	specfem2D \
	tomography/postprocess_sensitivity_kernels \
	auxiliaries \
	$(EMPTY_MACRO)

# default targets for the pure Fortran version
DEFAULT = \
	xmeshfem2D \
	xspecfem2D \
	xadj_seismogram \
	xcheck_quality_external_mesh \
	xconvolve_source_timefunction \
	$(EMPTY_MACRO)


default: $(DEFAULT)

all: default auxiliaries tomography/postprocess_sensitivity_kernels


ifdef CLEAN
clean:
	@echo "cleaning by CLEAN"
	-rm -f $(foreach dir, $(CLEAN), $($(dir)_OBJECTS) $($(dir)_MODULES) $($(dir)_SHARED_OBJECTS) $($(dir)_TARGETS))
	-rm -f ${E}/*__genmod.*
	-rm -f ${O}/*__genmod.*
else
clean:
	@echo "cleaning all"
	-rm -f $(foreach dir, $(SUBDIRS), $($(dir)_OBJECTS) $($(dir)_MODULES) $($(dir)_TARGETS))
	-rm -f ${E}/*__genmod.*
	-rm -f ${O}/*__genmod.*
endif

realclean: clean
ifeq (${SCOTCH_BUNDLED},1)
	@echo "cleaning bundled Scotch in directory: ${SCOTCH_DIR}/src"
	$(MAKE) -C ${SCOTCH_DIR}/src realclean
endif
	-rm -rf $E/* $O/*


help:
	@echo "usage: make [executable]"
	@echo ""
	@echo "supported executables:"
	@echo "    xspecfem2D"
	@echo "    xmeshfem2D"
	@echo "    xadj_seismogram"
	@echo "    xcheck_quality_external_mesh"
	@echo "    xconvolve_source_timefunction"
	@echo ""


.PHONY: all default backup clean realclean help

#######################################


# Get dependencies and rules for building stuff
include $(patsubst %, ${S_TOP}/src/%/rules.mk, $(SUBDIRS))

#######################################

##
## Shortcuts
##

# Shortcut for: <prog>/<xprog> -> bin/<xprog>
define target_shortcut
$(patsubst $E/%, %, $(1)): $(1)
.PHONY: $(patsubst $E/%, %, $(1))
$(patsubst $E/x%, %, $(1)): $(1)
.PHONY: $(patsubst $E/x%, %, $(1))
endef

# Shortcut for: dir -> src/dir/<targets in here>
define shortcut
$(1): $($(1)_TARGETS)
.PHONY: $(1)
$$(foreach target, $$(filter $E/%,$$($(1)_TARGETS)), $$(eval $$(call target_shortcut,$$(target))))
endef

$(foreach dir, $(SUBDIRS), $(eval $(call shortcut,$(dir))))


# Other old shortcuts

mesh : $E/xmeshfem2D
spec : $E/xspecfem2D
.PHONY: mesh spec
