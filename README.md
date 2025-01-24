<br>

<p align="center">
     <img src="https://age.apache.org/age-manual/master/_static/logo.png" width="30%" height="30%">
</p>
<br>

<h3 align="center">
    <a href="https://age.apache.org/age-manual/master/_static/logo.png" target="_blank">
        <img src="https://age.apache.org/age-manual/master/_static/logo.png" height="25" height="30% alt="Apache AGE style="margin: 0 0 -3px 0">
    </a>
    <a href="https://age.apache.org/age-manual/master/_static/logo.png" target="_blank">
    </a>
     is a leading multi-model graph database </h3>
     
</h3>

<h3 align="center">Graph Processing & Analytics for Relational Databases</h3>

<br>


</br>



<p align="center">                                                                                                    
  <a href="https://github.com/apache/age/blob/master/LICENSE">
    <img src="https://img.shields.io/github/license/apache/age"/>
  </a>
  &nbsp;
  <a href="https://github.com/apache/age/releases">
    <img src="https://img.shields.io/badge/Release-v1.5.0-FFA500?labelColor=gray&style=flat&link=https://github.com/apache/age/releases"/>
  </a>
  &nbsp;
  <a href="https://www.postgresql.org/docs/16/index.html">
    <img src="https://img.shields.io/badge/Version-Postgresql 17-00008B?labelColor=gray&style=flat&link=https://www.postgresql.org/docs/16/index.html"/>
  </a>
  &nbsp;
  <a href="https://github.com/apache/age/issues">
    <img src="https://img.shields.io/github/issues/apache/age"/>
  </a>
  &nbsp;
  <a href="https://github.com/apache/age/network/members">
    <img src="https://img.shields.io/github/forks/apache/age"/>
  </a>
  &nbsp;
  <a href="https://github.com/apache/age/stargazers">
    <img src="https://img.shields.io/github/stars/apache/age"/>
  </a>
  &nbsp;
  <a href="https://discord.gg/EuK6EEg3k7">
    <img src="https://img.shields.io/discord/1022177873127280680.svg?label=discord&style=flat&color=5a66f6"></a>
</p>

## Purpose of this project
Sometimes I need to work with graph databases on Windows, Neo4j is heavy and I prefer PostgreSQL and PGVector as a whole solution. Then Apache AGE is a good choice.

However, Apache AGE does not provide Windows support, the tutorials are based on WSL. I want to make it simple for Windows users to use the Apache AGE Along with PostgreSQL and PGVector.

This project is a fork of the [Apache AGE](https://github.com/apache/age) project. To provide Windows distribution for the Apache AGE project.
It will provide a binary package for Windows, along with PostgreSQL 17 and PGVector 0.8.0.

# Release
Please navigate to the [release page](https://github.com/ShanGor/apache-age-windows/releases/tag/PG17%2Fv1.5.0-rc0) and download:
- `pgsql17-mingw64-x86_64-min.7z`: Minimal runnable package, without ssl or icu support etc. (The package compiled with those dependencies are not included)
- `pgsql17-mingw64-x86-dependencies.7z`: Dependencies to enable the PG with icu, ssl, lz4, zstd etc. You can extract the files to the PG folder, or just copy those DLL files.
- `pgAdmin4-8.14-windows-x86_64.7z`: `pgAdmin 4` v8.14 for Windows.

# How to start it?
- Download the pre-compiled package from the release page. 
- Your folder structure might be look like this
  ```
  c:\swdtools\PostgreSQL
   |- data
   |- pgAdmin4
   |- pgsql17-dependencies
   |- pgsql17-mingw
  ```
- Initialize the database: 
  ```
  set PATH=C:\swdtools\PostgreSQL\pgsql17-mingw\bin;C:\swdtools\PostgreSQL\pgsql17-dependencies\bin;%PATH%
  initdb -D C:\swdtools\PostgreSQL\data
  ```
- Prepare some start / stop script like this:
  - start-pg-server.bat
  ```
  @echo off
  set PGDIR=%~dp0
  set PATH_O=%PATH%
  set PATH=%PGDIR%\pgsql17-mingw\bin;%PGDIR%\pgsql17-dependencies\bin;%PATH%
  pg_ctl -D %PGDIR%\data -l %PGDIR%\pg.log start
  set PATH=%PATH_O%
  echo on
  ```
  - stop-pg-server.bat
  ```
  @echo off
  set PGDIR=%~dp0
  set PATH_O=%PATH%
  set PATH=%PGDIR%\pgsql17-mingw\bin;%PGDIR%\pgsql17-dependencies\bin;%PATH%
  pg_ctl -D %PGDIR%\data stop
  set PATH=%PATH_O%
  echo on
  @pause
  ```

# Steps to compile if you want to do it on your machine:
1. Install MSYS2-MINGW64, and install the following packages:
   ```
   pacman -Syu
   pacman --needed -S git mingw-w64-x86_64-gcc base-devel bison flex mingw-w64-x86_64-minizip mingw-w64-x86_64-zlib mingw-w64-x86_64-icu
   pacman -S mingw64/mingw-w64-x86_64-openssl
   ```
2. Install PostgreSQL
   `pacman -S mingw-w64-x86_64-postgresql`
3. Checkout the source and build:
  ```
  git clone --branch "PG17/Windows" https://github.com/ShanGor/apache-age-windows.git
  cd apache-age-windows
  make install
  ```
4. If failed at cypher_gram_def.h, change below VARIABLE names:
  ```
  STRING -> CG_STRING
  CHAR -> CG_CHAR
  DELETE -> CG_DELETE
  OPTIONAL -> CG_OPTIONAL
  IN -> CG_IN
  ```

<h2><img height="20" src="/img/community.svg">&nbsp;&nbsp;Contributing</h2>

You can improve ongoing efforts or initiate new ones by sending pull requests to [this repository](https://github.com/apache/age).
Also, you can learn from the code review process, how to merge pull requests, and from code style compliance to documentation by visiting the [Apache AGE official site - Developer Guidelines](https://age.apache.org/contribution/guide).
Send all your comments and inquiries to the user mailing list, users@age.apache.org.
