#
# Copyright 2020, DornerWorks
#
# This software may be distributed and modified according to the terms of
# the BSD 2-Clause license. Note that NO WARRANTY is provided.
# See "LICENSE_BSD2.txt" for details.
#
# @TAG(DORNERWORKS_BSD)
#

cmake_minimum_required(VERSION 3.8.2)

project(test)

# Include CAmkES VM helper functions
include("../../vm/camkes_vm_helpers.cmake")
include("../../vm-linux/vm-linux-helpers.cmake")
include("../../dw-vm-helpers/dw_camkes_vm_helpers.cmake")

# Declare VM components: Init0
DeclareCAmkESVM(Init0)

# Get Default Linux VM files
GetDefaultLinuxKernelFile(kernel_file)

# Decompress Linux Kernel image and add to file server
DecompressLinuxKernel(extract_linux_kernel decompressed_kernel ${kernel_file})
AddToFileServer("bzimage" ${decompressed_kernel} DEPENDS extract_linux_kernel)

AddCamkesCPPFlag(cpp_flags
  CONFIG_VARS LibSatadriversAHCIEnable
  )

DeclareCAmkESVMRootServer(test.camkes
    CPP_FLAGS ${cpp_flags}
    )
