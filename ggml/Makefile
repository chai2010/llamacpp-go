default:
	mkdir -p build
	cd build && cmake .. && make

	cd build && ./simple-backend
	cd build && ./simple-ctx

clean:
	-rm -rf build
