.. This file was auto-generated using jldoc.py.
   DO NOT EDIT THIS FILE.
   Edit the original Julia source code with the documentation.

ZipFile --- A Julia package for reading/writing ZIP archive files
=================================================================

.. module::  ZipFile
	:synopsis:  A Julia package for reading/writing ZIP archive files

This package provides support for reading and writing ZIP archives in Julia.
Install it via the Julia package manager using ``Pkg.add("ZipFile")``.

The ZIP file format is described in
http://www.pkware.com/documents/casestudies/APPNOTE.TXT

Example
-------

Write a new ZIP file::

	using ZipFile

	w = ZipFile.Writer("example.zip");
	f = ZipFile.addfile(w, "hello.txt");
	write(f, "hello world!\n");
	f = ZipFile.addfile(w, "julia.txt", method=ZipFile.Deflate);
	write(f, "Julia\n"^5);
	close(w)

Read and print out the contents of a ZIP file::

	r = ZipFile.Reader("example.zip");
	for f in r.files
		println("Filename: $(f.name)")
		write(readall(f));
	end
	close(r)

Output::

	Filename: hello.txt
	hello world!
	Filename: julia.txt
	Julia
	Julia
	Julia
	Julia
	Julia


Exports
-------
.. code-block:: julia

	export read, eof, write, close, mtime, position, show

Constants
---------
.. code-block:: julia

	const Store = @compat UInt16(0)   # Compression method that does no compression
	const Deflate = @compat UInt16(8) # Deflate compression method

Type ReadableFile
-----------------
.. code-block:: julia

	type ReadableFile <: IO
		name :: UTF8String          # filename
		method :: UInt16            # compression method
		dostime :: UInt16           # modification time in MS-DOS format
		dosdate :: UInt16           # modification date in MS-DOS format
		crc32 :: UInt32             # CRC32 of uncompressed data
		compressedsize :: UInt32    # file size after compression
		uncompressedsize :: UInt32  # size of uncompressed file
	end

Type Reader
-----------
.. code-block:: julia

	type Reader
		files :: Vector{ReadableFile} # ZIP file entries that can be read concurrently
		comment :: UTF8String         # ZIP file comment
	end

.. function::  Reader(io::IO)

Read a ZIP file from io.

.. function::  Reader(filename::AbstractString)

Read a ZIP file from the file named filename.

Type WritableFile
-----------------
.. code-block:: julia

	type WritableFile <: IO
		name :: UTF8String          # filename
		method :: UInt16            # compression method
		dostime :: UInt16           # modification time in MS-DOS format
		dosdate :: UInt16           # modification date in MS-DOS format
		crc32 :: UInt32             # CRC32 of uncompressed data
		compressedsize :: UInt32    # file size after compression
		uncompressedsize :: UInt32  # size of uncompressed file
	end

Type Writer
-----------
.. code-block:: julia

	type Writer
		files :: Vector{WritableFile} # files (being) written
	end

.. function::  Writer(io::IO)

Create a new ZIP file that will be written to io.

.. function::  Writer(filename::AbstractString)

Create a new ZIP file that will be written to the file named filename.

Function show
-------------
.. function::  show(io::IO, f::@compat Union{ReadableFile, WritableFile})

Print out a summary of f in a human-readable format.

.. function::  show(io::IO, rw::@compat Union{Reader, Writer})

Print out a summary of rw in a human-readable format.

Function mtime
--------------
.. function::  mtime(f::@compat Union{ReadableFile, WritableFile})

Returns the modification time of f as seconds since epoch.

Function close
--------------
.. function::  close(r::Reader)

Close the underlying IO instance if it was opened by Reader.
User is still responsible for closing the IO instance if it was passed to Reader.

.. function::  close(w::Writer)

Finish writing the ZIP file and close the underlying IO instance if it was opened by Writer.
User is still responsible for closing the IO instance if it was passed to Writer.

.. function::  close(f::WritableFile)

Flush the file f into the ZIP file.

.. function::  close(f::ReadableFile)

A no-op provided for completeness.

Function read
-------------
.. function::  read{T}(f::ReadableFile, a::Array{T})

Read data into a. Throws EOFError if a cannot be filled in completely.

.. function::  read(f::ReadableFile, ::Type{UInt8})

Read and return a byte from f. Throws EOFError if there is no more byte to read.

Function eof
------------
.. function::  eof(f::ReadableFile)

Returns true if and only if we have reached the end of file f.

Function addfile
----------------
.. function::  addfile(w::Writer, name::AbstractString; method::Integer=Store, mtime::Float64=-1.0)

Add a new file named name into the ZIP file writer w, and return the
WritableFile for the new file. We don't allow concurrrent writes,
thus the file previously added using this function will be closed.
Method specifies the compression method that will be used, and mtime is the
modification time of the file.

Function position
-----------------
.. function::  position(f::WritableFile)

Returns the current position in file f.

.. function::  position(f::ReadableFile)

Returns the current position in file f.

Function write
--------------
.. function::  write(f::WritableFile, p::Ptr, nb::Integer)

Write nb elements located at p into f.

.. function::  write{T}(w::WritableFile, a::Array{T})

Writer the content of a into w.

.. function::  write(w::WritableFile, b::UInt8)

Writer the byte b in w.

