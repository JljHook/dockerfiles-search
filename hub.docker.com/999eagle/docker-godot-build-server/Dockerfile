FROM 999eagle/docker-godot-build-mono-glue

RUN	cd /build/godot-src/src && \
	scons platform=server target=release_debug tools=yes bits=64 module_mono_enabled=yes
