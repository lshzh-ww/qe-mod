# Copyright (C) 2001-2016 Quantum ESPRESSO group
#
# This program is free software; you can redistribute it and/or
# modify it under the terms of the GNU General Public License
# as published by the Free Software Foundation; either version 2
# of the License. See the file `License' in the root directory
# of the present distribution.

AC_INIT(deviceXlib, 2.0.0, , deviceXlib)

# Do not use Autoconf 2.69 
AC_PREREQ(2.64)
AC_CONFIG_MACRO_DIR([m4/])
m4_include([m4/x_ac_python.m4])
m4_include([m4/x_ac_qe_fft_aux.m4])
m4_include([m4/ax_check_compile_flag.m4])
m4_include([m4/x_ac_qe_cuda.m4])
m4_include([m4/acx_misc.m4])

# Initialize variables, filling with one comes from the environment...
X_AC_QE_DEFAULT_ENV()

# configure for current directory by default
if test "$topdir" = "" ; then topdir="`pwd`" ; fi

# check system type (no cross-compilation for now)
AC_CANONICAL_BUILD

# Checking Architecture...
X_AC_QE_ARCH()

# Add all needed -D options to try_dflags
try_dflags=""

# "-I../include" is required by IOTK ...
try_iflags="-I\$(TOPDIR)/include -I../include/"

# Checking archiver...
X_AC_QE_AR()

# Checking OpenMP...
X_AC_QE_OPENMP()

# Checking MPIF90...
X_AC_QE_MPIF90()

# Check environ...
X_AC_QE_ENVIRON()

# Checking CC...
X_AC_QE_CC()

# Checking F90...
X_AC_QE_F90()

# Checking preprocessor...
X_AC_QE_CPP()

# Checking linker...
X_AC_QE_LD()

# Checking CUDA...
X_AC_QE_CUDA()

# Checking F90 rule...
X_AC_QE_F90RULE()

AC_LANG_PUSH(Fortran 77)
F77=$f90 # use Fortran 90 actually
FFLAGS="$test_fflags"
LDFLAGS="$test_ldflags"

# Checking BLAS...
X_AC_QE_BLAS()

# Checking LAPACK...
X_AC_QE_LAPACK()

# Checking for FFT...
#X_AC_QE_FFT()
#X_AC_QE_FFT_AUX()

# Checking for IBM MASS library...
X_AC_QE_MASS()

# check for MPI library...
X_AC_QE_MPI()

# Setting ScaLAPACK...
X_AC_QE_SCALAPACK()

# Setting ELPA...
X_AC_QE_ELPA()

# Enable XML ...
X_AC_QE_XML()

# Setting HDF5...
X_AC_QE_HDF5()

# Checking SIGNAL...
X_AC_QE_SIGNAL()

# Checking for ranlib...
X_AC_QE_RANLIB()

# Checking wget or curl...
X_AC_QE_WGET()

# Checking python support
X_AC_PYTHON()

# Show dflags before adding $(MANUAL_DFLAGS) and adapt to XLF (if needed)
if test "$dflags" = "" ; then dflags="$try_dflags" ; fi
echo setting DFLAGS... $try_dflags

# xlf compilers (AIX and powerpc) want comma-separated -D directives
X_AC_QE_AIX_DFLAGS()

if test "$iflags" = "" ; then iflags="$try_iflags" ; fi
echo setting IFLAGS... $iflags

# export additional settings to generated files
AC_SUBST(dflags)
AC_SUBST(fdflags)
AC_SUBST(iflags)
AC_SUBST(ld_libs)
AC_SUBST(topdir)
AC_SUBST(host)           # <AF>
AC_SUBST(arch)           # <AF>
AC_SUBST(extlib_flags)

AC_CONFIG_HEADERS([include/c_defs.h:include/c_defs.h.in])
AC_CONFIG_FILES([include/configure.h:include/configure.h.in]) # AF

#AC_CONFIG_FILES(Makefile.cpu)
AC_CONFIG_FILES(make.inc)
AC_CONFIG_FILES(configure.msg)

AC_OUTPUT

# AF
# create also f_defs.h with a (not so) dirty trick
#
if test -e include/c_defs.h
then
   test -e include/f_defs.h && rm include/f_defs.h
   grep "#define " include/c_defs.h > include/f_defs.h
fi
# AF

# final messages
sed '/@delete@/d' configure.msg
echo configure: success

