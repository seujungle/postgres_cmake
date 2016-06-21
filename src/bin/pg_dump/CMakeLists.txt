include_directories(BEFORE
 	"${PROJECT_SOURCE_DIR}/src/include/libpq"
 	"${PROJECT_SOURCE_DIR}/src/interfaces/libpq"
 	"${PROJECT_SOURCE_DIR}/src/bin/pg_dump"
	${ZLIB_INCLUDE_DIRS}
)

add_library(libdump STATIC
	pg_backup_archiver.c
	pg_backup_db.c
	pg_backup_custom.c
	pg_backup_null.c
	pg_backup_tar.c
	pg_backup_directory.c
	pg_backup_utils.c
	parallel.c
	compress_io.c
	dumputils.c
)

add_executable(pg_dump
	pg_dump.c
	common.c
	pg_dump_sort.c
)

target_link_libraries(pg_dump
	libdump
	pgfeutils
	pgcommon
	port
	pq
	${ZLIB_LIBRARIES}
)

add_executable(pg_restore
	pg_restore.c
)

target_link_libraries(pg_restore
	libdump
	pgfeutils
	pgcommon
	port
	pq
	${ZLIB_LIBRARIES}
)

add_executable(pg_dumpall
	pg_dumpall.c
	dumputils.c
)

target_link_libraries(pg_dumpall
	libdump
	pgfeutils
	pgcommon
	port
	pq
	${ZLIB_LIBRARIES}
)

install(TARGETS pg_dump pg_restore pg_dumpall
		RUNTIME DESTINATION ${PGBINDIR}
		LIBRARY DESTINATION ${LIBDIR})

if(ENABLE_NLS)
	MAKE_MO(pg_dump "cs;de;es;fr;it;ja;ko;pl;pt_BR;ru;zh_CN")
endif()