# =====================================================================
#               automount - mount service for block nodes
# =====================================================================
# One instance per block node dmdevfs reports, started by libsystemd - see
# README.md. Only a main(): the work is done by libautomount, which is
# loaded once for all instances.
set(DMOD_MODULE_NAME        automount)
set(DMOD_AUTHOR_NAME        "Patryk Kubiak")
# 4 kB: the thread that mounts also runs what the mount sets going - dmvfs reports it to the modules implementing
# dmfsi's _mount_changed dif in that thread, and libsystemd loads <mount point>/cfg/services there and starts its
# units (a module loaded from the new file system, with its dependencies): 2.6 kB measured, 2 kB overflows.
set(DMOD_STACK_SIZE         4096)
set(DMOD_PRIORITY           0)
set(DMOD_DMR_PATH           ${CMAKE_CURRENT_SOURCE_DIR}/automount.dmr)

dmod_add_executable(${DMOD_MODULE_NAME} ${DMOD_MODULE_VERSION}
    src/main.c
)

target_link_libraries(${DMOD_MODULE_NAME} libautomount_if)
