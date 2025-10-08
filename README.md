~$  export GITHUB_USERNAME= Kinpach
 ~$ cd ${GITHUB_USERNAME}/workspace
 ~/ Kinpach/workspace$ pushd .
~/ Kinpach/workspace ~/ Kinpach/workspace
 ~/ Kinpach/workspace$ source scripts/activate
 ~/ Kinpach/workspace$ git clone https://github.com/${GITHUB_USERNAME}/TimpLab07 TimpLab08
Клонирование в «TimpLab08»...
remote: Enumerating objects: 95, done.
remote: Counting objects: 100% (95/95), done.
remote: Compressing objects: 100% (47/47), done.
remote: Total 95 (delta 40), reused 87 (delta 37), pack-reused 0 (from 0)
Получение объектов: 100% (95/95), 43.73 КиБ | 422.00 КиБ/с, готово.
Определение изменений: 100% (40/40), готово.
 ~/ Kinpach/workspace$  cd TimpLab08
 ~/ Kinpach/workspace/TimpLab08$ git submodule update --init
Подмодуль «third-party/gtest» (https://github.com/google/googletest) зарегистрирован по пути «third-party/gtest»
Клонирование в «/home/litclick/ Kinpach/workspace/TimpLab08/third-party/gtest»...
Submodule path 'third-party/gtest': checked out 'e2239ee6043f73722e7aa812a459f54a28552929'
 ~/ Kinpach/workspace/TimpLab08$ git remote remove origin
 ~/ Kinpach/workspace/TimpLab08$ git remote add origin https://github.com/${GITHUB_USERNAME}/TimpLab08
 ~/ Kinpach/workspace/TimpLab08$ cat > Dockerfile <<EOF
> FROM ubuntu:18.04
> EOF
 ~/ Kinpach/workspace/TimpLab08$ cat >> Dockerfile <<EOF
> COPY . print/
WORKDIR print
> EOF
 ~/ Kinpach/workspace/TimpLab08$ cat >> Dockerfile <<EOF
> RUN cmake -H. -B_build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=_install
RUN cmake --build _build
RUN cmake --build _build --target install
> EOF
 ~/ Kinpach/workspace/TimpLab08$ cat >> Dockerfile <<EOF
> ENV LOG_PATH /home/logs/log.txt
> EOF

 ~$ cd  Kinpach/workspace/TimpLab08
 ~/ Kinpach/workspace/TimpLab08$ cat >> Dockerfile <<EOF
> VOLUME /home/logs
> EOF
 ~/ Kinpach/workspace/TimpLab08$ cat >> Dockerfile <<EOF
> WORKDIR _install/bin
> EOF
 ~/ Kinpach/workspace/TimpLab08$ cat >> Dockerfile <<EOF
> ENTRYPOINT ./demo
> EOF
 ~/ Kinpach/workspace/TimpLab08$ docker build -t logger .
Команда «docker» не найдена, но может быть установлена с помощью:
sudo snap install docker         # version 27.5.1, or
sudo apt  install docker.io      # version 26.1.3-0ubuntu1~24.04.1
sudo apt  install podman-docker  # version 4.9.3+ds1-1ubuntu0.2
См. 'snap info docker', чтобы посмотреть дополнительные версии.
 ~/ Kinpach/workspace/TimpLab08$ sudo snap install docker  
[sudo] пароль для litclick: 
docker 27.5.1 from Canonical✓ installed
 ~/ Kinpach/workspace/TimpLab08$ docker build -t logger .
ERROR: permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Head "http://%2Fvar%2Frun%2Fdocker.sock/_ping": dial unix /var/run/docker.sock: connect: permission denied
 ~/ Kinpach/workspace/TimpLab08$ getent group docker
 ~/ Kinpach/workspace/TimpLab08$ groups
litclick adm cdrom sudo dip plugdev users lpadmin
 ~/ Kinpach/workspace/TimpLab08$ sudo usermod -aG docker litclick
usermod: группа «docker» не существует
 ~/ Kinpach/workspace/TimpLab08$ sudo groupadd docker
 ~/ Kinpach/workspace/TimpLab08$ sudo usermod -aG docker litclick
 ~/ Kinpach/workspace/TimpLab08$ groups
litclick adm cdrom sudo dip plugdev users lpadmin

 ~$ cd  Kinpach/workspace/TimpLab08
 ~/ Kinpach/workspace/TimpLab08$ docker build -t logger .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  19.69MB
Step 1/10 : FROM ubuntu:18.04
18.04: Pulling from library/ubuntu
7c457f213c76: Pull complete 
Digest: sha256:152dc042452c496007f07ca9127571cb9c29697f42acbfad72324b2bb2e43c98
Status: Downloaded newer image for ubuntu:18.04
 ---> f9a80a55f492
Step 2/10 : COPY . print/
 ---> f29536edc378
Step 3/10 : WORKDIR print
 ---> Running in 611c899f01b1
 ---> Removed intermediate container 611c899f01b1
 ---> 6f48ec71fbd3
Step 4/10 : RUN cmake -H. -B_build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=_install
 ---> Running in 365bdf960b63
/bin/sh: 1: cmake: not found
The command '/bin/sh -c cmake -H. -B_build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=_install' returned a non-zero code: 127
 ~/ Kinpach/workspace/TimpLab08$ cmake --version
cmake version 3.28.3

CMake suite maintained and supported by Kitware (kitware.com/cmake).
 ~/ Kinpach/workspace/TimpLab08$ cat > Dockerfile <<EOF
> FROM ubuntu:20.04
> EOF
 ~/ Kinpach/workspace/TimpLab08$ docker build -t logger .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  19.69MB
Step 1/1 : FROM ubuntu:20.04
20.04: Pulling from library/ubuntu
13b7e930469f: Pull complete 
Digest: sha256:8feb4d8ca5354def3d8fce243717141ce31e2c428701f6682bd2fafe15388214
Status: Downloaded newer image for ubuntu:20.04
 ---> b7bab04fd9aa
Successfully built b7bab04fd9aa
Successfully tagged logger:latest
 ~/ Kinpach/workspace/TimpLab08$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
<none>       <none>    6f48ec71fbd3   4 minutes ago   82.5MB
logger       latest    b7bab04fd9aa   4 weeks ago     72.8MB
ubuntu       20.04     b7bab04fd9aa   4 weeks ago     72.8MB
ubuntu       18.04     f9a80a55f492   23 months ago   63.2MB
 ~/ Kinpach/workspace/TimpLab08$  mkdir logs
 ~$ cd  Kinpach/workspace/TimpLab08
 ~/ Kinpach/workspace/TimpLab08$ docker build -t logger .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  19.69MB
Step 1/10 : FROM ubuntu:18.04
18.04: Pulling from library/ubuntu
7c457f213c76: Pull complete 
Digest: sha256:152dc042452c496007f07ca9127571cb9c29697f42acbfad72324b2bb2e43c98
Status: Downloaded newer image for ubuntu:18.04
 ---> f9a80a55f492
Step 2/10 : COPY . print/
 ---> f29536edc378
Step 3/10 : WORKDIR print
 ---> Running in 611c899f01b1
 ---> Removed intermediate container 611c899f01b1
 ---> 6f48ec71fbd3
Step 4/10 : RUN cmake -H. -B_build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=_install
 ---> Running in 365bdf960b63
/bin/sh: 1: cmake: not found
The command '/bin/sh -c cmake -H. -B_build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=_install' returned a non-zero code: 127
 ~/ Kinpach/workspace/TimpLab08$ cmake --version
cmake version 3.28.3

CMake suite maintained and supported by Kitware (kitware.com/cmake).
 ~/ Kinpach/workspace/TimpLab08$ cat > Dockerfile <<EOF
> FROM ubuntu:20.04
> EOF
 ~/ Kinpach/workspace/TimpLab08$ docker build -t logger .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  19.69MB
Step 1/1 : FROM ubuntu:20.04
20.04: Pulling from library/ubuntu
13b7e930469f: Pull complete 
Digest: sha256:8feb4d8ca5354def3d8fce243717141ce31e2c428701f6682bd2fafe15388214
Status: Downloaded newer image for ubuntu:20.04
 ---> b7bab04fd9aa
Successfully built b7bab04fd9aa
Successfully tagged logger:latest
 ~/ Kinpach/workspace/TimpLab08$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
<none>       <none>    6f48ec71fbd3   4 minutes ago   82.5MB
logger       latest    b7bab04fd9aa   4 weeks ago     72.8MB
ubuntu       20.04     b7bab04fd9aa   4 weeks ago     72.8MB
ubuntu       18.04     f9a80a55f492   23 months ago   63.2MB
 ~/ Kinpach/workspace/TimpLab08$  mkdir logs
 ~/ Kinpach/workspace/TimpLab08$ docker run -it -v "$(pwd)/logs/:/home/logs/" logger
text1
text2
text3
<C-D>
root@16d40953e4d1:/# 
root@16d40953e4d1:/# 
root@16d40953e4d1:/# 
root@16d40953e4d1:/# 
root@16d40953e4d1:/# 
root@16d40953e4d1:/# text1: команда не найдена
text2: команда не найдена
text3: команда не найдена
bash: синтаксическая ошибка рядом с неожиданным маркером «newline»
 ~/ Kinpach/workspace/TimpLab08$ docker run -it -v "$(pwd)/logs/:/home/logs/" logger
text1
text2
text3
root@7527863f1944:/# exit
text1: команда не найдена
text2: команда не найдена
text3: команда не найдена
 ~/ Kinpach/workspace/TimpLab08$ ^C
 ~/ Kinpach/workspace/TimpLab08$ docker run -it -v "$(pwd)/logs/:/home/logs/" logger
root@c5e283c7e9f6:/# text1
bash: text1: command not found
root@c5e283c7e9f6:/# text1
bash: text1: command not found
root@c5e283c7e9f6:/# exit
 ~/ Kinpach/workspace/TimpLab08$ docker run -it -v "$(pwd)/logs/:/home/logs/" logger
root@281e6327ca0d:/# exit
 ~/ Kinpach/workspace/TimpLab08$ docker inspect logger
[
    {
        "Id": "sha256:b7bab04fd9aa0c771e5720bf0cc7cbf993fd6946645983d9096126e5af45d713",
        "RepoTags": [
            "logger:latest",
            "ubuntu:20.04"
        ],
        "RepoDigests": [
            "ubuntu@sha256:8feb4d8ca5354def3d8fce243717141ce31e2c428701f6682bd2fafe15388214"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2025-04-08T10:42:48.947155373Z",
        "DockerVersion": "24.0.7",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash"
            ],
            "Image": "sha256:a4f4aac46bc2829ab75a33c79ebe803110dd26e79bbd1dce17875492019b230c",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.opencontainers.image.ref.name": "ubuntu",
                "org.opencontainers.image.version": "20.04"
            }
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 72813617,
        "GraphDriver": {
            "Data": {
                "MergedDir": "/var/snap/docker/common/var-lib-docker/overlay2/a9a243a1f43ec5a10f27f716086edd1fd9bb6970b7361c0d3d9c36a06d4df9de/merged",
                "UpperDir": "/var/snap/docker/common/var-lib-docker/overlay2/a9a243a1f43ec5a10f27f716086edd1fd9bb6970b7361c0d3d9c36a06d4df9de/diff",
                "WorkDir": "/var/snap/docker/common/var-lib-docker/overlay2/a9a243a1f43ec5a10f27f716086edd1fd9bb6970b7361c0d3d9c36a06d4df9de/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:470b66ea5123c93b0d5606e4213bf9e47d3d426b640d32472e4ac213186c4bb6"
            ]
        },
        "Metadata": {
            "LastTagTime": "2025-05-11T23:45:58.383836055+03:00"
        }
    }
]
 ~/ Kinpach/workspace/TimpLab08$  cat logs/log.txt
cat: logs/log.txt: Нет такого файла или каталога


 ~/ Kinpach/workspace/TimpLab08$ docker run -it -v "$(pwd)/logs:/home/logs" logger bash -c "cat > /home/logs/log.txt"
text1
text
text3
 ~/ Kinpach/workspace/TimpLab08$ ^C
 ~/ Kinpach/workspace/TimpLab08$ cat logs/log.txt
text1
text
text3
 ~/ Kinpach/workspace/TimpLab08$  gsed -i 's/TimpLab07/TimpLab08/g' README.md
Команда «gsed» не найдена. Возможно, вы имели в виду:
  команда 'msed' из deb-пакета mblaze (1.1-1)
  команда 'gsd' из deb-пакета python3-gsd (3.0.1-3build1)
  команда 'sed' из deb-пакета sed (4.9-2)
  команда 'gted' из deb-пакета gnome-text-editor (46.3-0ubuntu2)
  команда 'gsec' из deb-пакета firebird3.0-utils (3.0.11.33637.ds4-2ubuntu2)
  команда 'ssed' из deb-пакета ssed (3.62-8)
  команда 'gsnd' из deb-пакета ghostscript (10.02.1~dfsg1-0ubuntu7.5)
  команда 'gsad' из deb-пакета gsad (22.8.0-1)
Попробуйте: sudo apt install <имя_deb-пакета>
 ~/ Kinpach/workspace/TimpLab08$ sed -i 's/TimpLab07/TimpLab08/g' README.md
 ~/ Kinpach/workspace/TimpLab08$ vim .travis.yml
 ~/ Kinpach/workspace/TimpLab08$ vim .travis.yml
 ~/ Kinpach/workspace/TimpLab08$ git add Dockerfile
 ~/ Kinpach/workspace/TimpLab08$ git add .travis.yml
 ~/ Kinpach/workspace/TimpLab08$ git commit -m"adding Dockerfile"
[master da0f64c] adding Dockerfile
 2 files changed, 14 insertions(+), 12 deletions(-)
 create mode 100644 Dockerfile
 ~/ Kinpach/workspace/TimpLab08$  git push origin master
Username for 'https://github.com':  Kinpach
Password for 'https:// Kinpach@github.com': 
Перечисление объектов: 99, готово.
Подсчет объектов: 100% (99/99), готово.
При сжатии изменений используется до 8 потоков
Сжатие объектов: 100% (47/47), готово.
Запись объектов: 100% (99/99), 44.06 КиБ | 14.69 МиБ/с, готово.
Всего 99 (изменений 41), повторно использовано 94 (изменений 40), повторно использовано пакетов 0
remote: Resolving deltas: 100% (41/41), done.
To https://github.com/ Kinpach/TimpLab08
 * [new branch]      master -> master
 ~/ Kinpach/workspace/TimpLab08$ 


 ~$ ls
 BMSTU           snap           Видео         Музыка          Шаблоны
  Kinpach   tasks          Документы     непонятно
 projects       'visual code'   Загрузки      Общедоступные
 reports         бандит         Изображения  'Рабочий стол'
 ~$ cd  Kinpach
 ~/ Kinpach$ cd workspace
 ~/ Kinpach/workspace$ cd TimpLab08
 ~/ Kinpach/workspace/TimpLab08$ git submodule update --init
 ~/ Kinpach/workspace/TimpLab08$ git remote remove origin
 ~/ Kinpach/workspace/TimpLab08$ git remote add origin https://github.com/${GITHUB_USERNAME}/TimpLab08

 ~/ Kinpach/workspace/TimpLab08$ docker build -t logger .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon   31.1MB
Step 1/12 : FROM ubuntu:20.04
 ---> b7bab04fd9aa
Step 2/12 : RUN apt update
 ---> Using cache
 ---> 08af04f05ded
Step 3/12 : RUN apt install -yy gcc g++ cmake
 ---> Using cache
 ---> 837eccc68fe2
Step 4/12 : COPY . print/
 ---> 015c59c57062
Step 5/12 : WORKDIR print
 ---> Running in 205baf1b94d9
 ---> Removed intermediate container 205baf1b94d9
 ---> 8922af37f50b
Step 6/12 : RUN cmake -H. -B_build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=_install
 ---> Running in 7d3fc65f96c5
-- [hunter] Initializing Hunter workspace (23f1b5a0acffae50fda423388c843a8e7b6e1eb0)
-- [hunter]   https://github.com/cpp-pm/hunter/archive/v0.23.308.tar.gz
-- [hunter]   -> /root/.hunter/_Base/Download/Hunter/0.23.308/23f1b5a
-- The C compiler identification is GNU 9.4.0
-- The CXX compiler identification is GNU 9.4.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- [hunter] Calculating Toolchain-SHA1
-- [hunter] Calculating Config-SHA1
-- [hunter] HUNTER_ROOT: /root/.hunter
-- [hunter] [ Hunter-ID: 23f1b5a | Toolchain-ID: f845a29 | Config-ID: bf2be25 ]
-- [hunter] GTEST_ROOT: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Install (ver.: 1.11.0)
-- [hunter] Building GTest
loading initial cache file /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/cache.cmake
loading initial cache file /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/args.cmake
-- The C compiler identification is GNU 9.4.0
-- The CXX compiler identification is GNU 9.4.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Build
Scanning dependencies of target GTest-Release
[  6%] Creating directories for 'GTest-Release'
[ 12%] Performing download step (download, verify and extract) for 'GTest-Release'
-- Downloading...
   dst='/root/.hunter/_Base/Download/GTest/1.11.0/7b100bb/release-1.11.0.tar.gz'
   timeout='none'
-- Using src='https://github.com/google/googletest/archive/release-1.11.0.tar.gz'
-- [download 1% complete]
-- [download 4% complete]
-- [download 5% complete]
-- [download 6% complete]
-- [download 7% complete]
-- [download 8% complete]
-- [download 9% complete]
-- [download 11% complete]
-- [download 12% complete]
-- [download 13% complete]
-- [download 14% complete]
-- [download 15% complete]
-- [download 16% complete]
-- [download 18% complete]
-- [download 19% complete]
-- [download 20% complete]
-- [download 21% complete]
-- [download 22% complete]
-- [download 23% complete]
-- [download 24% complete]
-- [download 25% complete]
-- [download 26% complete]
-- [download 27% complete]
-- [download 28% complete]
-- [download 29% complete]
-- [download 30% complete]
-- [download 31% complete]
-- [download 32% complete]
-- [download 33% complete]
-- [download 34% complete]
-- [download 35% complete]
-- [download 36% complete]
-- [download 37% complete]
-- [download 39% complete]
-- [download 41% complete]
-- [download 43% complete]
-- [download 44% complete]
-- [download 45% complete]
-- [download 46% complete]
-- [download 48% complete]
-- [download 49% complete]
-- [download 50% complete]
-- [download 51% complete]
-- [download 52% complete]
-- [download 53% complete]
-- [download 54% complete]
-- [download 55% complete]
-- [download 57% complete]
-- [download 58% complete]
-- [download 60% complete]
-- [download 62% complete]
-- [download 63% complete]
-- [download 64% complete]
-- [download 65% complete]
-- [download 66% complete]
-- [download 67% complete]
-- [download 68% complete]
-- [download 69% complete]
-- [download 71% complete]
-- [download 75% complete]
-- [download 76% complete]
-- [download 77% complete]
-- [download 78% complete]
-- [download 79% complete]
-- [download 80% complete]
-- [download 81% complete]
-- [download 82% complete]
-- [download 83% complete]
-- [download 85% complete]
-- [download 87% complete]
-- [download 89% complete]
-- [download 90% complete]
-- [download 91% complete]
-- [download 92% complete]
-- [download 93% complete]
-- [download 94% complete]
-- [download 95% complete]
-- [download 96% complete]
-- [download 97% complete]
-- [download 98% complete]
-- [download 99% complete]
-- [download 100% complete]
-- verifying file...
       file='/root/.hunter/_Base/Download/GTest/1.11.0/7b100bb/release-1.11.0.tar.gz'
-- Downloading... done
-- extracting...
     src='/root/.hunter/_Base/Download/GTest/1.11.0/7b100bb/release-1.11.0.tar.gz'
     dst='/root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Source'
-- extracting... [tar xfz]
-- extracting... [analysis]
-- extracting... [rename]
-- extracting... [clean up]
-- extracting... done
[ 18%] No patch step for 'GTest-Release'
[ 25%] No update step for 'GTest-Release'
[ 31%] Performing configure step for 'GTest-Release'
loading initial cache file /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/cache.cmake
loading initial cache file /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/args.cmake
-- The C compiler identification is GNU 9.4.0
-- The CXX compiler identification is GNU 9.4.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Could NOT find Python (missing: Python_EXECUTABLE Interpreter) 
-- Looking for pthread.h
-- Looking for pthread.h - found
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Failed
-- Looking for pthread_create in pthreads
-- Looking for pthread_create in pthreads - not found
-- Looking for pthread_create in pthread
-- Looking for pthread_create in pthread - found
-- Found Threads: TRUE  
-- Configuring done
-- Generating done
-- Build files have been written to: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Build/GTest-Release-prefix/src/GTest-Release-build
[ 37%] Performing build step for 'GTest-Release'
Scanning dependencies of target gtest
[ 12%] Building CXX object googletest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
[ 25%] Linking CXX static library ../lib/libgtest.a
[ 25%] Built target gtest
Scanning dependencies of target gtest_main
Scanning dependencies of target gmock
[ 37%] Building CXX object googletest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 50%] Building CXX object googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 62%] Linking CXX static library ../lib/libgtest_main.a
[ 62%] Built target gtest_main
[ 75%] Linking CXX static library ../lib/libgmock.a
[ 75%] Built target gmock
Scanning dependencies of target gmock_main
[ 87%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[100%] Linking CXX static library ../lib/libgmock_main.a
[100%] Built target gmock_main
[ 43%] Performing install step for 'GTest-Release'
[ 25%] Built target gtest
[ 50%] Built target gmock
[ 75%] Built target gmock_main
[100%] Built target gtest_main
Install the project...
-- Install configuration: "Release"
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/internal
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/internal/custom
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/internal/custom/gmock-generated-actions.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/internal/custom/gmock-matchers.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/internal/custom/README.md
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/internal/custom/gmock-port.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/internal/gmock-port.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/internal/gmock-internal-utils.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/internal/gmock-pp.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/gmock-actions.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/gmock-more-matchers.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/gmock-function-mocker.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/gmock-spec-builders.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/gmock.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/gmock-matchers.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/gmock-nice-strict.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/gmock-cardinalities.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/gmock-more-actions.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/libgmock.a
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/libgmock_main.a
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/pkgconfig/gmock.pc
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/pkgconfig/gmock_main.pc
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/cmake/GTest/GTestTargets.cmake
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/cmake/GTest/GTestTargets-release.cmake
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/cmake/GTest/GTestConfigVersion.cmake
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/cmake/GTest/GTestConfig.cmake
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/gtest-message.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/gtest-spi.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/gtest.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/gtest-port.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/gtest-death-test-internal.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/gtest-param-util.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/gtest-port-arch.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/gtest-type-util.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/custom
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/custom/gtest.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/custom/gtest-port.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/custom/gtest-printers.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/custom/README.md
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/gtest-internal.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/gtest-string.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/gtest-filepath.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/gtest-printers.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/gtest-param-test.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/gtest_prod.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/gtest_pred_impl.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/gtest-typed-test.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/gtest-death-test.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/gtest-matchers.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/gtest-test-part.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/libgtest.a
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/libgtest_main.a
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/pkgconfig/gtest.pc
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/pkgconfig/gtest_main.pc
loading initial cache file /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/args.cmake
[ 50%] Completed 'GTest-Release'
[ 50%] Built target GTest-Release
Scanning dependencies of target GTest-Debug
[ 56%] Creating directories for 'GTest-Debug'
[ 62%] Performing download step (download, verify and extract) for 'GTest-Debug'
-- verifying file...
       file='/root/.hunter/_Base/Download/GTest/1.11.0/7b100bb/release-1.11.0.tar.gz'
-- File already exists and hash match (skip download):
  file='/root/.hunter/_Base/Download/GTest/1.11.0/7b100bb/release-1.11.0.tar.gz'
  SHA1='7b100bb68db8df1060e178c495f3cbe941c9b058'
-- extracting...
     src='/root/.hunter/_Base/Download/GTest/1.11.0/7b100bb/release-1.11.0.tar.gz'
     dst='/root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Source'
-- extracting... [tar xfz]
-- extracting... [analysis]
-- extracting... [rename]
-- extracting... [clean up]
-- extracting... done
[ 68%] No patch step for 'GTest-Debug'
[ 75%] No update step for 'GTest-Debug'
[ 81%] Performing configure step for 'GTest-Debug'
loading initial cache file /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/cache.cmake
loading initial cache file /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/args.cmake
-- The C compiler identification is GNU 9.4.0
-- The CXX compiler identification is GNU 9.4.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Could NOT find Python (missing: Python_EXECUTABLE Interpreter) 
-- Looking for pthread.h
-- Looking for pthread.h - found
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Failed
-- Looking for pthread_create in pthreads
-- Looking for pthread_create in pthreads - not found
-- Looking for pthread_create in pthread
-- Looking for pthread_create in pthread - found
-- Found Threads: TRUE  
-- Configuring done
-- Generating done
-- Build files have been written to: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Build/GTest-Debug-prefix/src/GTest-Debug-build
[ 87%] Performing build step for 'GTest-Debug'
Scanning dependencies of target gtest
[ 12%] Building CXX object googletest/CMakeFiles/gtest.dir/src/gtest-all.cc.o
[ 25%] Linking CXX static library ../lib/libgtestd.a
[ 25%] Built target gtest
Scanning dependencies of target gtest_main
Scanning dependencies of target gmock
[ 37%] Building CXX object googletest/CMakeFiles/gtest_main.dir/src/gtest_main.cc.o
[ 50%] Building CXX object googlemock/CMakeFiles/gmock.dir/src/gmock-all.cc.o
[ 62%] Linking CXX static library ../lib/libgtest_maind.a
[ 62%] Built target gtest_main
[ 75%] Linking CXX static library ../lib/libgmockd.a
[ 75%] Built target gmock
Scanning dependencies of target gmock_main
[ 87%] Building CXX object googlemock/CMakeFiles/gmock_main.dir/src/gmock_main.cc.o
[100%] Linking CXX static library ../lib/libgmock_maind.a
[100%] Built target gmock_main
[ 93%] Performing install step for 'GTest-Debug'
[ 25%] Built target gtest
[ 50%] Built target gmock
[ 75%] Built target gmock_main
[100%] Built target gtest_main
Install the project...
-- Install configuration: "Debug"
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/internal
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/internal/custom
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/internal/custom/gmock-generated-actions.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/internal/custom/gmock-matchers.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/internal/custom/README.md
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/internal/custom/gmock-port.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/internal/gmock-port.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/internal/gmock-internal-utils.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/internal/gmock-pp.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/gmock-actions.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/gmock-more-matchers.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/gmock-function-mocker.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/gmock-spec-builders.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/gmock.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/gmock-matchers.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/gmock-nice-strict.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/gmock-cardinalities.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gmock/gmock-more-actions.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/libgmockd.a
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/libgmock_maind.a
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/pkgconfig/gmock.pc
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/pkgconfig/gmock_main.pc
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/cmake/GTest/GTestTargets.cmake
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/cmake/GTest/GTestTargets-debug.cmake
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/cmake/GTest/GTestConfigVersion.cmake
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/cmake/GTest/GTestConfig.cmake
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/gtest-message.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/gtest-spi.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/gtest.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/gtest-port.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/gtest-death-test-internal.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/gtest-param-util.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/gtest-port-arch.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/gtest-type-util.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/custom
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/custom/gtest.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/custom/gtest-port.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/custom/gtest-printers.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/custom/README.md
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/gtest-internal.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/gtest-string.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/internal/gtest-filepath.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/gtest-printers.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/gtest-param-test.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/gtest_prod.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/gtest_pred_impl.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/gtest-typed-test.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/gtest-death-test.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/gtest-matchers.h
-- Up-to-date: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/include/gtest/gtest-test-part.h
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/libgtestd.a
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/libgtest_maind.a
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/pkgconfig/gtest.pc
-- Installing: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/Install/lib/pkgconfig/gtest_main.pc
loading initial cache file /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest/args.cmake
[100%] Completed 'GTest-Debug'
[100%] Built target GTest-Debug
-- [hunter] Build step successful (dir: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Build/GTest)
-- [hunter] Cache saved: /root/.hunter/_Base/Cache/raw/975b193882aacc8b6f1e1198934397fba12800ad.tar.bz2
-- Found GTest: /root/.hunter/_Base/23f1b5a/f845a29/bf2be25/Install/lib/cmake/GTest/GTestConfig.cmake (found version "1.11.0")  
-- Configuring done
-- Generating done
-- Build files have been written to: /print/_build
 ---> Removed intermediate container 7d3fc65f96c5
 ---> 7d0d5ed07005
Step 7/12 : RUN cmake --build _build
 ---> Running in 5d2f8693941b
Scanning dependencies of target print
[ 25%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[ 50%] Linking CXX static library libprint.a
[ 50%] Built target print
Scanning dependencies of target demo
[ 75%] Building CXX object CMakeFiles/demo.dir/demo/main.cpp.o
[100%] Linking CXX executable demo
[100%] Built target demo
 ---> Removed intermediate container 5d2f8693941b
 ---> 7ec74b922a06
Step 8/12 : RUN cmake --build _build --target install
 ---> Running in 2836adcc40a1
[ 50%] Built target print
[100%] Built target demo
Install the project...
-- Install configuration: "Release"
-- Installing: /print/_install/lib/libprint.a
-- Installing: /print/_install/include
-- Installing: /print/_install/include/print.hpp
-- Installing: /print/_install/cmake/print-config.cmake
-- Installing: /print/_install/cmake/print-config-release.cmake
-- Installing: /print/_install/bin/demo
 ---> Removed intermediate container 2836adcc40a1
 ---> b440ecfa6823
Step 9/12 : ENV LOG_PATH /home/logs/log.txt
 ---> Running in 0c93c4a09837
 ---> Removed intermediate container 0c93c4a09837
 ---> 0fd7d22bb0ca
Step 10/12 : VOLUME /home/logs
 ---> Running in ba30ff3832ed
 ---> Removed intermediate container ba30ff3832ed
 ---> 0698d4ce0cae
Step 11/12 : WORKDIR _install/bin
 ---> Running in 999b16501380
 ---> Removed intermediate container 999b16501380
 ---> 5046e7269a9a
Step 12/12 : ENTRYPOINT ./demo
 ---> Running in bb5705abe5d5
 ---> Removed intermediate container bb5705abe5d5
 ---> ad05ccf65b29
Successfully built ad05ccf65b29
Successfully tagged logger:latest
 ~/ Kinpach/workspace/TimpLab08$  docker images
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
logger       latest    ad05ccf65b29   13 seconds ago   454MB
<none>       <none>    d6a08a876c08   56 minutes ago   422MB
<none>       <none>    6f48ec71fbd3   23 hours ago     82.5MB
ubuntu       20.04     b7bab04fd9aa   4 weeks ago      72.8MB
ubuntu       18.04     f9a80a55f492   23 months ago    63.2MB
 ~/ Kinpach/workspace/TimpLab08$  mkdir logs
mkdir: невозможно создать каталог «logs»: Файл существует
 ~/ Kinpach/workspace/TimpLab08$ docker inspect logger
[
    {
        "Id": "sha256:ad05ccf65b29a1a0f3b0c927d042df98f0c296233ad9ad49f3dca2f01f507d9b",
        "RepoTags": [
            "logger:latest"
        ],
        "RepoDigests": [],
        "Parent": "sha256:5046e7269a9ab3d43e83e8446c10b1b5f63f72be8eeea6eeb2a266929d4f40d5",
        "Comment": "",
        "Created": "2025-05-12T19:20:31.628939441Z",
        "DockerVersion": "27.5.1",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "LOG_PATH=/home/logs/log.txt"
            ],
            "Cmd": null,
            "Image": "sha256:5046e7269a9ab3d43e83e8446c10b1b5f63f72be8eeea6eeb2a266929d4f40d5",
            "Volumes": {
                "/home/logs": {}
            },
            "WorkingDir": "/print/_install/bin",
            "Entrypoint": [
                "/bin/sh",
                "-c",
                "./demo"
            ],
            "OnBuild": null,
            "Labels": {
                "org.opencontainers.image.ref.name": "ubuntu",
                "org.opencontainers.image.version": "20.04"
            }
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 454362950,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/snap/docker/common/var-lib-docker/overlay2/13c44622f47c43fd1338e6a6ddf8d44de4fd3d40774f53950e7cf09e7d535da1/diff:/var/snap/docker/common/var-lib-docker/overlay2/99a3583f7110a46a37dc8c431689315478ccf88b133c52f0bfecfcf9b8da5fa8/diff:/var/snap/docker/common/var-lib-docker/overlay2/f7eee5a4e190b71e201a94980de90468f164829bc02211b10015091c66ec4aaa/diff:/var/snap/docker/common/var-lib-docker/overlay2/4211b670b752d9fba89e5ee8e223b4dc4f0642a1a8dc220f8012e76f87af89ac/diff:/var/snap/docker/common/var-lib-docker/overlay2/ed5430cd3bd14929ebfd22ccf97771d54d73b429955b1f48cce671e55f0581d5/diff:/var/snap/docker/common/var-lib-docker/overlay2/a9a243a1f43ec5a10f27f716086edd1fd9bb6970b7361c0d3d9c36a06d4df9de/diff",
                "MergedDir": "/var/snap/docker/common/var-lib-docker/overlay2/f73787b9aef9fc2d2f061bf4f925b41a3304a25b3684887bd5fe638d64b8cb96/merged",
                "UpperDir": "/var/snap/docker/common/var-lib-docker/overlay2/f73787b9aef9fc2d2f061bf4f925b41a3304a25b3684887bd5fe638d64b8cb96/diff",
                "WorkDir": "/var/snap/docker/common/var-lib-docker/overlay2/f73787b9aef9fc2d2f061bf4f925b41a3304a25b3684887bd5fe638d64b8cb96/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:470b66ea5123c93b0d5606e4213bf9e47d3d426b640d32472e4ac213186c4bb6",
                "sha256:2ca3daac3e79399fe2c35a6e63d290e8a689f25949899b342ed613798ee619a8",
                "sha256:dc05a74f0f9422856c1d57d0a8014d0511a873f9343ed1be786f00c210d3c1e8",
                "sha256:0e29531873db5b3432fca2ec212dfb3285a9a19b5e65ccd5229350f1b4b90d75",
                "sha256:9b7372e5085fbedcf238269dad6f6ceb456c61a10b4c377226eb9e2f5e0744e9",
                "sha256:9556dfec37eaa0e2ed4f5e9f716f44781494334013060ac31e54afe42470d89d",
                "sha256:dccc806a31ea4536328f26c641de1ffc58df6603b44da34c7411f4b532d0890c"
            ]
        },
        "Metadata": {
            "LastTagTime": "2025-05-12T22:20:31.685618912+03:00"
        }
    }
]
 ~/ Kinpach/workspace/TimpLab08$ cat logs/log.txt
text1
text
text3
 ~/ Kinpach/workspace/TimpLab08$ git add Dockerfile
 ~/ Kinpach/workspace/TimpLab08$  git commit -m"adding Dockerfile"
[master d64e69b] adding Dockerfile
 1 file changed, 13 insertions(+)
 ~/ Kinpach/workspace/TimpLab08$ git push origin master
Username for 'https://github.com':  Kinpach
Password for 'https:// Kinpach@github.com': 
remote: Repository not found.
fatal: repository 'https://github.com//TimpLab08/' not found
 ~/ Kinpach/workspace/TimpLab08$ git remote remove origin
 ~/ Kinpach/workspace/TimpLab08$ export GITHUB_USERNAME= Kinpach
 ~/ Kinpach/workspace/TimpLab08$ git remote add origin https://github.com/${GITHUB_USERNAME}/TimpLab08
 ~/ Kinpach/workspace/TimpLab08$ git push origin master
Username for 'https://github.com':  Kinpach
Password for 'https:// Kinpach@github.com': 
To https://github.com/ Kinpach/TimpLab08
 ! [rejected]        master -> master (fetch first)
error: не удалось отправить некоторые ссылки в «https://github.com/ Kinpach/TimpLab08»
подсказка: Updates were rejected because the remote contains work that you do not
подсказка: have locally. This is usually caused by another repository pushing to
подсказка: the same ref. If you want to integrate the remote changes, use
подсказка: 'git pull' before pushing again.
подсказка: See the 'Note about fast-forwards' in 'git push --help' for details.
 ~/ Kinpach/workspace/TimpLab08$ 
git pull origin master
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
Распаковка объектов: 100% (3/3), 4.69 КиБ | 1.56 МиБ/с, готово.
Из https://github.com/ Kinpach/TimpLab08
 * branch            master     -> FETCH_HEAD
 * [новая ветка]     master     -> origin/master
подсказка: You have divergent branches and need to specify how to reconcile them.
подсказка: You can do so by running one of the following commands sometime before
подсказка: your next pull:
подсказка: 
подсказка:   git config pull.rebase false  # merge
подсказка:   git config pull.rebase true   # rebase
подсказка:   git config pull.ff only       # fast-forward only
подсказка: 
подсказка: You can replace "git config" with "git config --global" to set a default
подсказка: preference for all repositories. You can also pass --rebase, --no-rebase,
подсказка: or --ff-only on the command line to override the configured default per
подсказка: invocation.
fatal: Need to specify how to reconcile divergent branches.
 ~/ Kinpach/workspace/TimpLab08$ git push origin master
Username for 'https://github.com':  Kinpach
Password for 'https:// Kinpach@github.com': 
To https://github.com/ Kinpach/TimpLab08
 ! [rejected]        master -> master (non-fast-forward)
error: не удалось отправить некоторые ссылки в «https://github.com/ Kinpach/TimpLab08»
подсказка: Updates were rejected because the tip of your current branch is behind
подсказка: its remote counterpart. If you want to integrate the remote changes,
подсказка: use 'git pull' before pushing again.
подсказка: See the 'Note about fast-forwards' in 'git push --help' for details.
 ~/ Kinpach/workspace/TimpLab08$ git pull --rebase origin master
error: не удалось выполнить получение с перемещением: У вас есть непроиндексированные изменения.
error: Сделайте коммит или спрячьте их.
 ~/ Kinpach/workspace/TimpLab08$ git commit -m"adding Dockerfile"
Текущая ветка: master
Изменения, которые не в индексе для коммита:
  (используйте «git add <файл>...», чтобы добавить файл в индекс)
  (используйте «git restore <файл>...», чтобы отменить изменения в рабочем каталоге)
	изменено:      README.md

Неотслеживаемые файлы:
  (используйте «git add <файл>...», чтобы добавить в то, что будет включено в коммит)
	cmake/
	demo/
	logs/
	tools/

индекс пуст (используйте «git add» и/или «git commit -a»)
 ~/ Kinpach/workspace/TimpLab08$ git add .
 ~/ Kinpach/workspace/TimpLab08$ git status
Текущая ветка: master
Изменения, которые будут включены в коммит:
  (используйте «git restore --staged <файл>...», чтобы убрать из индекса)
	изменено:      README.md
	новый файл:    cmake/Hunter/config.cmake
	новый файл:    cmake/HunterGate.cmake
	новый файл:    demo/main.cpp
	новый файл:    logs/log.txt
	новый файл:    tools/polly/.gitignore
	новый файл:    tools/polly/.gitmodules
	новый файл:    tools/polly/.travis.yml
	новый файл:    tools/polly/CONTRIBUTING.md
	новый файл:    tools/polly/LICENSE
	новый файл:    tools/polly/analyze-cxx17.cmake
	новый файл:    tools/polly/analyze.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-16-armeabi-v7a-neon-clang-35-hid-sections.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-16-armeabi-v7a-neon-clang-35-hid.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-16-armeabi-v7a-neon-clang-35.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-16-armeabi-v7a-neon.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-16-x86-hid-sections.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-16-x86-hid.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-16-x86.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-19-armeabi-v7a-neon-c11.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-19-armeabi-v7a-neon-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-19-armeabi-v7a-neon-hid-sections-lto.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-19-armeabi-v7a-neon-hid-sections.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-19-armeabi-v7a-neon.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-21-arm64-v8a-clang-35.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-21-arm64-v8a-gcc-49-hid-sections.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-21-arm64-v8a-gcc-49-hid.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-21-arm64-v8a-gcc-49.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-21-arm64-v8a.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-21-armeabi-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-21-armeabi-v7a-neon-clang-35.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-21-armeabi-v7a-neon-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-21-armeabi-v7a-neon-hid-sections.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-21-armeabi-v7a-neon.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-21-armeabi-v7a.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-21-armeabi.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-21-mips-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-21-mips.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-21-mips64.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-21-x86-64-hid-sections.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-21-x86-64-hid.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-21-x86-64.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-21-x86-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-21-x86.cmake
	новый файл:    tools/polly/android-ndk-r10e-api-8-armeabi-v7a.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-16-armeabi-cxx14.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-16-armeabi-v7a-cxx14.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-16-armeabi-v7a-neon-clang-35-hid.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-16-armeabi-v7a-neon-clang-35.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-16-armeabi-v7a-neon-cxx14.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-16-armeabi-v7a-neon.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-16-armeabi-v7a.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-16-armeabi.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-16-x86-hid.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-16-x86.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-19-armeabi-v7a-neon.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-21-arm64-v8a-clang-35.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-21-arm64-v8a-gcc-49-hid.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-21-arm64-v8a-gcc-49.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-21-arm64-v8a.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-21-armeabi-v7a-neon-clang-35.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-21-armeabi-v7a-neon.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-21-armeabi-v7a.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-21-armeabi.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-21-mips.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-21-mips64.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-21-x86-64-hid.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-21-x86-64.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-21-x86.cmake
	новый файл:    tools/polly/android-ndk-r11c-api-8-armeabi-v7a.cmake
	новый файл:    tools/polly/android-ndk-r12b-api-19-armeabi-v7a-neon.cmake
	новый файл:    tools/polly/android-ndk-r13b-api-19-armeabi-v7a-neon.cmake
	новый файл:    tools/polly/android-ndk-r14-api-16-armeabi-v7a-neon-clang-hid-sections-lto.cmake
	новый файл:    tools/polly/android-ndk-r14-api-19-armeabi-v7a-neon-c11.cmake
	новый файл:    tools/polly/android-ndk-r14-api-19-armeabi-v7a-neon-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r14-api-19-armeabi-v7a-neon-clang.cmake
	новый файл:    tools/polly/android-ndk-r14-api-19-armeabi-v7a-neon-hid-sections-lto.cmake
	новый файл:    tools/polly/android-ndk-r14-api-21-arm64-v8a-clang-hid-sections-lto.cmake
	новый файл:    tools/polly/android-ndk-r14-api-21-arm64-v8a-neon-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r14-api-21-x86-64.cmake
	новый файл:    tools/polly/android-ndk-r14b-api-21-armeabi-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r14b-api-21-armeabi-v7a-neon-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r14b-api-21-mips-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r14b-api-21-x86-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r15c-api-16-armeabi-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r15c-api-16-armeabi-v7a-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r15c-api-16-armeabi-v7a-neon-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r15c-api-16-mips-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r15c-api-16-x86-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r15c-api-21-arm64-v8a-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r15c-api-21-arm64-v8a-neon-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r15c-api-21-armeabi-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r15c-api-21-armeabi-v7a-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r15c-api-21-armeabi-v7a-neon-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r15c-api-21-mips-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r15c-api-21-x86-64-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r15c-api-21-x86-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r15c-api-24-armeabi-v7a-neon-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r16b-api-16-armeabi-v7a-clang-libcxx14.cmake
	новый файл:    tools/polly/android-ndk-r16b-api-16-armeabi-v7a-thumb-clang-libcxx14.cmake
	новый файл:    tools/polly/android-ndk-r16b-api-16-x86-clang-libcxx14.cmake
	новый файл:    tools/polly/android-ndk-r16b-api-19-gcc-49-armeabi-v7a-neon-libcxx-hid-sections-lto.cmake
	новый файл:    tools/polly/android-ndk-r16b-api-21-arm64-v8a-neon-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r16b-api-21-arm64-v8a-neon-clang-libcxx14.cmake
	новый файл:    tools/polly/android-ndk-r16b-api-21-armeabi-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r16b-api-21-armeabi-clang-libcxx14.cmake
	новый файл:    tools/polly/android-ndk-r16b-api-21-armeabi-v7a-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r16b-api-21-armeabi-v7a-clang-libcxx14.cmake
	новый файл:    tools/polly/android-ndk-r16b-api-21-armeabi-v7a-neon-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r16b-api-21-armeabi-v7a-neon-clang-libcxx14.cmake
	новый файл:    tools/polly/android-ndk-r16b-api-21-x86-64-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r16b-api-21-x86-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r16b-api-24-arm64-v8a-clang-libcxx14.cmake
	новый файл:    tools/polly/android-ndk-r16b-api-24-armeabi-v7a-neon-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r16b-api-24-armeabi-v7a-neon-clang-libcxx14.cmake
	новый файл:    tools/polly/android-ndk-r16b-api-24-x86-64-clang-libcxx14.cmake
	новый файл:    tools/polly/android-ndk-r16b-api-24-x86-clang-libcxx14.cmake
	новый файл:    tools/polly/android-ndk-r17-api-16-armeabi-v7a-clang-libcxx14.cmake
	новый файл:    tools/polly/android-ndk-r17-api-16-x86-clang-libcxx14.cmake
	новый файл:    tools/polly/android-ndk-r17-api-19-armeabi-v7a-neon-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r17-api-19-armeabi-v7a-neon-hid-sections.cmake
	новый файл:    tools/polly/android-ndk-r17-api-21-arm64-v8a-neon-clang-libcxx14.cmake
	новый файл:    tools/polly/android-ndk-r17-api-21-x86-64-clang-libcxx14.cmake
	новый файл:    tools/polly/android-ndk-r17-api-24-arm64-v8a-clang-libcxx11.cmake
	новый файл:    tools/polly/android-ndk-r17-api-24-arm64-v8a-clang-libcxx14.cmake
	новый файл:    tools/polly/android-ndk-r18-api-24-arm64-v8a-clang-libcxx14.cmake
	новый файл:    tools/polly/android-ndk-r18b-api-16-armeabi-v7a-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r18b-api-21-arm64-v8a-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r18b-api-21-armeabi-v7a-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r18b-api-21-x86-64-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r18b-api-21-x86-clang-libcxx.cmake
	новый файл:    tools/polly/android-ndk-r18b-api-24-arm64-v8a-clang-libcxx11.cmake
	новый файл:    tools/polly/android-ndk-r18b-api-28-arm64-v8a-clang-libcxx11.cmake
	новый файл:    tools/polly/android-vc-ndk-r10e-api-19-arm-clang-3-6.cmake
	новый файл:    tools/polly/android-vc-ndk-r10e-api-19-arm-gcc-4-9.cmake
	новый файл:    tools/polly/android-vc-ndk-r10e-api-19-x86-clang-3-6.cmake
	новый файл:    tools/polly/android-vc-ndk-r10e-api-21-arm-clang-3-6.cmake
	новый файл:    tools/polly/appveyor.yml
	новый файл:    tools/polly/arm-openwrt-linux-muslgnueabi-cxx14.cmake
	новый файл:    tools/polly/arm-openwrt-linux-muslgnueabi.cmake
	новый файл:    tools/polly/bin/build.py
	новый файл:    tools/polly/bin/detail/__init__.py
	новый файл:    tools/polly/bin/detail/call.py
	новый файл:    tools/polly/bin/detail/cpack_generator.py
	новый файл:    tools/polly/bin/detail/create_archive.py
	новый файл:    tools/polly/bin/detail/create_framework.py
	новый файл:    tools/polly/bin/detail/generate_command.py
	новый файл:    tools/polly/bin/detail/get_nmake_environment.py
	новый файл:    tools/polly/bin/detail/ios_dev_root.py
	новый файл:    tools/polly/bin/detail/logging.py
	новый файл:    tools/polly/bin/detail/open_project.py
	новый файл:    tools/polly/bin/detail/osx_dev_root.py
	новый файл:    tools/polly/bin/detail/pack_command.py
	новый файл:    tools/polly/bin/detail/rmtree.py
	новый файл:    tools/polly/bin/detail/target.py
	новый файл:    tools/polly/bin/detail/test_command.py
	новый файл:    tools/polly/bin/detail/timer.py
	новый файл:    tools/polly/bin/detail/toolchain_name.py
	новый файл:    tools/polly/bin/detail/toolchain_table.py
	новый файл:    tools/polly/bin/detail/util.py
	новый файл:    tools/polly/bin/detail/verify_mingw_path.py
	новый файл:    tools/polly/bin/detail/verify_msys_path.py
	новый файл:    tools/polly/bin/detail/win32.py
	новый файл:    tools/polly/bin/install-ci-dependencies.py
	новый файл:    tools/polly/bin/polly
	новый файл:    tools/polly/bin/polly.bat
	новый файл:    tools/polly/bin/polly.py
	новый файл:    tools/polly/clang-5-cxx14.cmake
	новый файл:    tools/polly/clang-5-cxx17.cmake
	новый файл:    tools/polly/clang-5.cmake
	новый файл:    tools/polly/clang-cxx11.cmake
	новый файл:    tools/polly/clang-cxx14-pic.cmake
	новый файл:    tools/polly/clang-cxx14.cmake
	новый файл:    tools/polly/clang-cxx17-pic.cmake
	новый файл:    tools/polly/clang-cxx17.cmake
	новый файл:    tools/polly/clang-cxx20.cmake
	новый файл:    tools/polly/clang-fpic-hid-sections.cmake
	новый файл:    tools/polly/clang-fpic-static-std-cxx14.cmake
	новый файл:    tools/polly/clang-fpic-static-std.cmake
	новый файл:    tools/polly/clang-fpic.cmake
	новый файл:    tools/polly/clang-libcxx-fpic.cmake
	новый файл:    tools/polly/clang-libcxx.cmake
	новый файл:    tools/polly/clang-libcxx14-fpic.cmake
	новый файл:    tools/polly/clang-libcxx14.cmake
	новый файл:    tools/polly/clang-libcxx17-fpic.cmake
	новый файл:    tools/polly/clang-libcxx17-static.cmake
	новый файл:    tools/polly/clang-libcxx17.cmake
	новый файл:    tools/polly/clang-libcxx98.cmake
	новый файл:    tools/polly/clang-libstdcxx.cmake
	новый файл:    tools/polly/clang-lto.cmake
	новый файл:    tools/polly/clang-omp.cmake
	новый файл:    tools/polly/clang-tidy-libcxx.cmake
	новый файл:    tools/polly/clang-tidy.cmake
	новый файл:    tools/polly/compiler/cl.cmake
	новый файл:    tools/polly/compiler/clang-5.cmake
	новый файл:    tools/polly/compiler/clang-omp.cmake
	новый файл:    tools/polly/compiler/clang-tools.cmake
	новый файл:    tools/polly/compiler/clang.cmake
	новый файл:    tools/polly/compiler/egcc.cmake
	новый файл:    tools/polly/compiler/emscripten.cmake
	новый файл:    tools/polly/compiler/emscripten/glew/glewConfig.cmake
	новый файл:    tools/polly/compiler/emscripten/glfw3/glfw3Config.cmake
	новый файл:    tools/polly/compiler/gcc-5.cmake
	новый файл:    tools/polly/compiler/gcc-6.cmake
	новый файл:    tools/polly/compiler/gcc-7.cmake
	новый файл:    tools/polly/compiler/gcc-8.cmake
	новый файл:    tools/polly/compiler/gcc-cross-compile-raspberry-pi.cmake
	новый файл:    tools/polly/compiler/gcc-cross-compile-simple-layout.cmake
	новый файл:    tools/polly/compiler/gcc-cross-compile.cmake
	новый файл:    tools/polly/compiler/gcc.cmake
	новый файл:    tools/polly/compiler/gcc48.cmake
	новый файл:    tools/polly/compiler/xcode.cmake
	новый файл:    tools/polly/custom-libcxx.cmake
	новый файл:    tools/polly/cxx11.cmake
	новый файл:    tools/polly/cxx17.cmake
	новый файл:    tools/polly/cygwin.cmake
	новый файл:    tools/polly/default.cmake
	новый файл:    tools/polly/docs/Makefile
	новый файл:    tools/polly/docs/_fixme/Building-libcxx.md
	новый файл:    tools/polly/docs/_fixme/Jenkins-(build-bot,-PR).md
	новый файл:    tools/polly/docs/_fixme/Jenkins-(build-bot,-integration).md
	новый файл:    tools/polly/docs/_fixme/Jenkins-(creating-user-on-linux-ubuntu).md
	новый файл:    tools/polly/docs/_fixme/Jenkins-(creating-user-on-mac).md
	новый файл:    tools/polly/docs/_fixme/Jenkins.md
	новый файл:    tools/polly/docs/_fixme/Toolchain-list.md
	новый файл:    tools/polly/docs/_fixme/Travis-CI-AppVeyor-support-table.md
	новый файл:    tools/polly/docs/_fixme/Used-variables.md
	новый файл:    tools/polly/docs/conf.py
	новый файл:    tools/polly/docs/index.rst
	новый файл:    tools/polly/docs/jenkins.sh
	новый файл:    tools/polly/docs/make.sh
	новый файл:    tools/polly/docs/requirements.txt
	новый файл:    tools/polly/docs/screens/ios-team-id.png
	новый файл:    tools/polly/docs/spelling.txt
	новый файл:    tools/polly/docs/toolchains.rst
	новый файл:    tools/polly/docs/toolchains/android.rst
	новый файл:    tools/polly/docs/toolchains/android/developer-notes.rst
	новый файл:    tools/polly/docs/toolchains/android/old.rst
	новый файл:    tools/polly/docs/toolchains/clang-omp.rst
	новый файл:    tools/polly/docs/toolchains/gcc-musl.rst
	новый файл:    tools/polly/docs/toolchains/ios.rst
	новый файл:    tools/polly/docs/toolchains/ios/bundle-id.rst
	новый файл:    tools/polly/docs/toolchains/ios/errors/polly_ios_bundle_identifier.rst
	новый файл:    tools/polly/docs/toolchains/ios/errors/polly_ios_development_team.rst
	новый файл:    tools/polly/docs/toolchains/ios/errors/signing-request-development-team.rst
	новый файл:    tools/polly/docs/toolchains/ios/screens/01_new_project.png
	новый файл:    tools/polly/docs/toolchains/ios/screens/02_single_view_app.png
	новый файл:    tools/polly/docs/toolchains/ios/screens/03_project_options.png
	новый файл:    tools/polly/docs/toolchains/ios/screens/04_bundle_identifier.png
	новый файл:    tools/polly/docs/toolchains/ios/screens/05_run_app.png
	новый файл:    tools/polly/docs/toolchains/ios/screens/bad_bundle_id.png
	новый файл:    tools/polly/docs/toolchains/linux-mingw-w64.rst
	новый файл:    tools/polly/docs/toolchains/raspberry-pi.rst
	новый файл:    tools/polly/emscripten-cxx11.cmake
	новый файл:    tools/polly/emscripten-cxx14.cmake
	новый файл:    tools/polly/emscripten-cxx17.cmake
	новый файл:    tools/polly/examples/01-executable/CMakeLists.txt
	новый файл:    tools/polly/examples/01-executable/main.cpp
	новый файл:    tools/polly/examples/02-library/CMakeLists.txt
	новый файл:    tools/polly/examples/02-library/foo.cpp
	новый файл:    tools/polly/examples/02-library/main.cpp
	новый файл:    tools/polly/examples/03-shared-link/CMakeLists.txt
	новый файл:    tools/polly/examples/03-shared-link/boo.cpp
	новый файл:    tools/polly/examples/03-shared-link/foo.cpp
	новый файл:    tools/polly/examples/03-shared-link/main.cpp
	новый файл:    tools/polly/examples/04-framework/CMakeLists.txt
	новый файл:    tools/polly/examples/04-framework/foo.cpp
	новый файл:    tools/polly/find/FindLibcxx.cmake
	новый файл:    tools/polly/flags/32bit.cmake
	новый файл:    tools/polly/flags/arm-linux-gnueabihf.cmake
	новый файл:    tools/polly/flags/bitcode.cmake
	новый файл:    tools/polly/flags/c11.cmake
	новый файл:    tools/polly/flags/clang-tidy.cmake
	новый файл:    tools/polly/flags/cxx11.cmake
	новый файл:    tools/polly/flags/cxx14.cmake
	новый файл:    tools/polly/flags/cxx17-gnu.cmake
	новый файл:    tools/polly/flags/cxx17.cmake
	новый файл:    tools/polly/flags/cxx20.cmake
	новый файл:    tools/polly/flags/cxx98.cmake
	новый файл:    tools/polly/flags/data-sections.cmake
	новый файл:    tools/polly/flags/fconcepts.cmake
	новый файл:    tools/polly/flags/fpic.cmake
	новый файл:    tools/polly/flags/function-sections.cmake
	новый файл:    tools/polly/flags/gnuxx11.cmake
	новый файл:    tools/polly/flags/gold.cmake
	новый файл:    tools/polly/flags/hardfloat.cmake
	новый файл:    tools/polly/flags/hidden.cmake
	новый файл:    tools/polly/flags/ios_nocodesign.cmake
	новый файл:    tools/polly/flags/lld.cmake
	новый файл:    tools/polly/flags/lto.cmake
	новый файл:    tools/polly/flags/mtune_cortex-a15.cmake
	новый файл:    tools/polly/flags/neon-vfpv4.cmake
	новый файл:    tools/polly/flags/neon.cmake
	новый файл:    tools/polly/flags/openwrt.cmake
	новый файл:    tools/polly/flags/sanitize_address.cmake
	новый файл:    tools/polly/flags/sanitize_leak.cmake
	новый файл:    tools/polly/flags/sanitize_memory.cmake
	новый файл:    tools/polly/flags/sanitize_thread.cmake
	новый файл:    tools/polly/flags/static-std.cmake
	новый файл:    tools/polly/flags/static.cmake
	новый файл:    tools/polly/flags/vs-cxx14.cmake
	новый файл:    tools/polly/flags/vs-cxx17.cmake
	новый файл:    tools/polly/flags/vs-mt.cmake
	новый файл:    tools/polly/flags/vs-nonpermissive.cmake
	новый файл:    tools/polly/flags/vs-platform-win32.cmake
	новый файл:    tools/polly/flags/vs-platform-x64.cmake
	новый файл:    tools/polly/flags/vs-version-14-11.cmake
	новый файл:    tools/polly/flags/vs-z7.cmake
	новый файл:    tools/polly/flags/vs-zw.cmake
	новый файл:    tools/polly/gcc-32bit-pic.cmake
	новый файл:    tools/polly/gcc-32bit.cmake
	новый файл:    tools/polly/gcc-4-8-c11.cmake
	новый файл:    tools/polly/gcc-4-8-pic-hid-sections-cxx11-c11.cmake
	новый файл:    tools/polly/gcc-4-8-pic-hid-sections.cmake
	новый файл:    tools/polly/gcc-4-8-pic.cmake
	новый файл:    tools/polly/gcc-4-8.cmake
	новый файл:    tools/polly/gcc-5-cxx14-c11.cmake
	новый файл:    tools/polly/gcc-5-pic-hid-sections-lto.cmake
	новый файл:    tools/polly/gcc-5-pic-hid-sections.cmake
	новый файл:    tools/polly/gcc-5.cmake
	новый файл:    tools/polly/gcc-6-32bit-cxx14.cmake
	новый файл:    tools/polly/gcc-7-cxx11-pic.cmake
	новый файл:    tools/polly/gcc-7-cxx14-pic.cmake
	новый файл:    tools/polly/gcc-7-cxx14.cmake
	новый файл:    tools/polly/gcc-7-cxx17-concepts.cmake
	новый файл:    tools/polly/gcc-7-cxx17-gnu.cmake
	новый файл:    tools/polly/gcc-7-cxx17-pic.cmake
	новый файл:    tools/polly/gcc-7-cxx17.cmake
	новый файл:    tools/polly/gcc-7-pic-hid-sections-lto.cmake
	новый файл:    tools/polly/gcc-7.cmake
	новый файл:    tools/polly/gcc-8-cxx14-fpic.cmake
	новый файл:    tools/polly/gcc-8-cxx14.cmake
	новый файл:    tools/polly/gcc-8-cxx17-concepts.cmake
	новый файл:    tools/polly/gcc-8-cxx17-fpic.cmake
	новый файл:    tools/polly/gcc-8-cxx17.cmake
	новый файл:    tools/polly/gcc-c11.cmake
	новый файл:    tools/polly/gcc-cxx14-c11.cmake
	новый файл:    tools/polly/gcc-cxx17-c11.cmake
	новый файл:    tools/polly/gcc-cxx98.cmake
	новый файл:    tools/polly/gcc-gold.cmake
	новый файл:    tools/polly/gcc-hid-fpic.cmake
	новый файл:    tools/polly/gcc-hid.cmake
	новый файл:    tools/polly/gcc-lto.cmake
	новый файл:    tools/polly/gcc-musl.cmake
	новый файл:    tools/polly/gcc-ninja.cmake
	новый файл:    tools/polly/gcc-pic-hid-sections-lto.cmake
	новый файл:    tools/polly/gcc-pic-hid-sections.cmake
	новый файл:    tools/polly/gcc-pic.cmake
	новый файл:    tools/polly/gcc-static-std.cmake
	новый файл:    tools/polly/gcc-static.cmake
	новый файл:    tools/polly/gcc.cmake
	новый файл:    tools/polly/ios-10-0-arm64-dep-8-0-hid-sections.cmake
	новый файл:    tools/polly/ios-10-0-arm64.cmake
	новый файл:    tools/polly/ios-10-0-armv7.cmake
	новый файл:    tools/polly/ios-10-0-dep-8-0-hid-sections.cmake
	новый файл:    tools/polly/ios-10-0-wo-armv7s.cmake
	новый файл:    tools/polly/ios-10-0.cmake
	новый файл:    tools/polly/ios-10-1-arm64-dep-8-0-hid-sections.cmake
	новый файл:    tools/polly/ios-10-1-arm64.cmake
	новый файл:    tools/polly/ios-10-1-armv7.cmake
	новый файл:    tools/polly/ios-10-1-dep-8-0-hid-sections.cmake
	новый файл:    tools/polly/ios-10-1-dep-8-0-libcxx-hid-sections-lto.cmake
	новый файл:    tools/polly/ios-10-1-dep-8-0-libcxx-hid-sections.cmake
	новый файл:    tools/polly/ios-10-1-wo-armv7s.cmake
	новый файл:    tools/polly/ios-10-1.cmake
	новый файл:    tools/polly/ios-10-2-dep-9-3-arm64.cmake
	новый файл:    tools/polly/ios-10-2-dep-9-3-armv7.cmake
	новый файл:    tools/polly/ios-10-2.cmake
	новый файл:    tools/polly/ios-10-3-arm64.cmake
	новый файл:    tools/polly/ios-10-3-armv7.cmake
	новый файл:    tools/polly/ios-10-3-dep-8-0-bitcode.cmake
	новый файл:    tools/polly/ios-10-3-dep-9-0-bitcode.cmake
	новый файл:    tools/polly/ios-10-3-dep-9-3-i386-armv7.cmake
	новый файл:    tools/polly/ios-10-3-dep-9-3-x86-64-arm64.cmake
	новый файл:    tools/polly/ios-10-3-lto.cmake
	новый файл:    tools/polly/ios-10-3.cmake
	новый файл:    tools/polly/ios-11-0-dep-9-0-bitcode-cxx11.cmake
	новый файл:    tools/polly/ios-11-0-dep-9-0-device-bitcode-cxx11.cmake
	новый файл:    tools/polly/ios-11-0-dep-9-0-x86-64-arm64-bitcode-cxx11.cmake
	новый файл:    tools/polly/ios-11-0.cmake
	новый файл:    tools/polly/ios-11-1-dep-9-0-bitcode-cxx11.cmake
	новый файл:    tools/polly/ios-11-1-dep-9-0-device-bitcode-cxx11.cmake
	новый файл:    tools/polly/ios-11-2-dep-9-0-device-bitcode-cxx11.cmake
	новый файл:    tools/polly/ios-11-2-dep-9-0-device-bitcode-nocxx.cmake
	новый файл:    tools/polly/ios-11-2-dep-9-3-arm64-armv7.cmake
	новый файл:    tools/polly/ios-11-3-dep-9-0-arm64.cmake
	новый файл:    tools/polly/ios-11-3-dep-9-0-device-bitcode-cxx11.cmake
	новый файл:    tools/polly/ios-11-3-dep-9-0-device-bitcode-cxx17.cmake
	новый файл:    tools/polly/ios-11-3-dep-9-0-device-bitcode-nocxx.cmake
	новый файл:    tools/polly/ios-11-3-dep-9-0-device-bitcode.cmake
	новый файл:    tools/polly/ios-11-3-dep-9-3-arm64-armv7.cmake
	новый файл:    tools/polly/ios-11-4-dep-8-0-arm64-armv7-hid-sections-lto-cxx11.cmake
	новый файл:    tools/polly/ios-11-4-dep-8-0-arm64-hid-sections-lto-cxx11.cmake
	новый файл:    tools/polly/ios-11-4-dep-9-0-device-bitcode-cxx11.cmake
	новый файл:    tools/polly/ios-11-4-dep-9-0-device-bitcode-nocxx.cmake
	новый файл:    tools/polly/ios-11-4-dep-9-3-arm64-armv7.cmake
	новый файл:    tools/polly/ios-11-4-dep-9-3-arm64-hid-sections-lto-cxx11.cmake
	новый файл:    tools/polly/ios-11-4-dep-9-3-arm64.cmake
	новый файл:    tools/polly/ios-11-4-dep-9-3-armv7.cmake
	новый файл:    tools/polly/ios-11-4-dep-9-3.cmake
	новый файл:    tools/polly/ios-11-4-dep-9-4-arm64.cmake
	новый файл:    tools/polly/ios-12-0-dep-11-0-arm64.cmake
	новый файл:    tools/polly/ios-12-0-dep-9-0-device-bitcode-cxx11.cmake
	новый файл:    tools/polly/ios-12-1-dep-11-0-arm64.cmake
	новый файл:    tools/polly/ios-12-1-dep-12-0-arm64-cxx17.cmake
	новый файл:    tools/polly/ios-12-1-dep-9-0-device-bitcode-cxx14.cmake
	новый файл:    tools/polly/ios-12-1-dep-9-0-device-bitcode-cxx17.cmake
	новый файл:    tools/polly/ios-12-1-dep-9-3-arm64-bitcode.cmake
	новый файл:    tools/polly/ios-12-1-dep-9-3-arm64.cmake
	новый файл:    tools/polly/ios-12-1-dep-9-3-armv7.cmake
	новый файл:    tools/polly/ios-12-1-dep-9-3-x86-64-arm64.cmake
	новый файл:    tools/polly/ios-12-1-dep-9-3.cmake
	новый файл:    tools/polly/ios-12-2-dep-9-3-arm64.cmake
	новый файл:    tools/polly/ios-12-3-dep-9-3-arm64.cmake
	новый файл:    tools/polly/ios-13-0-dep-10-0-arm64-bitcode-cxx17.cmake
	новый файл:    tools/polly/ios-13-0-dep-11-0-arm64-bitcode-cxx17.cmake
	новый файл:    tools/polly/ios-13-0-dep-9-3-arm64-bitcode.cmake
	новый файл:    tools/polly/ios-13-0-dep-9-3-arm64.cmake
	новый файл:    tools/polly/ios-13-2-dep-10-0-arm64-bitcode-cxx17.cmake
	новый файл:    tools/polly/ios-13-2-dep-9-3-arm64-bitcode.cmake
	новый файл:    tools/polly/ios-13-2-dep-9-3-arm64.cmake
	новый файл:    tools/polly/ios-13-2-dep-9-3-armv7.cmake
	новый файл:    tools/polly/ios-13-2-dep-9-3-armv7s.cmake
	новый файл:    tools/polly/ios-13-2-dep-9-3-device-bitcode-cxx14.cmake
	новый файл:    tools/polly/ios-13-2-dep-9-3-device-cxx14.cmake
	новый файл:    tools/polly/ios-13-3-dep-9-3-arm64.cmake
	новый файл:    tools/polly/ios-13-3-dep-9-3-armv7.cmake
	новый файл:    tools/polly/ios-13-3-dep-9-3-armv7s.cmake
	новый файл:    tools/polly/ios-13-3-dep-9-3-device-bitcode-cxx14.cmake
	новый файл:    tools/polly/ios-13-3-dep-9-3-device-cxx14.cmake
	новый файл:    tools/polly/ios-13-4-dep-9-3-arm64.cmake
	новый файл:    tools/polly/ios-13-4-dep-9-3-armv7.cmake
	новый файл:    tools/polly/ios-13-4-dep-9-3-armv7s.cmake
	новый файл:    tools/polly/ios-13-4-dep-9-3-device-bitcode-cxx14.cmake
	новый файл:    tools/polly/ios-13-4-dep-9-3-device-cxx14.cmake
	новый файл:    tools/polly/ios-13-5-dep-9-3-arm64.cmake
	новый файл:    tools/polly/ios-13-5-dep-9-3-armv7.cmake
	новый файл:    tools/polly/ios-13-5-dep-9-3-armv7s.cmake
	новый файл:    tools/polly/ios-13-5-dep-9-3-device-bitcode-cxx14.cmake
	новый файл:    tools/polly/ios-13-5-dep-9-3-device-cxx14.cmake
	новый файл:    tools/polly/ios-13-6-dep-9-3-arm64.cmake
	новый файл:    tools/polly/ios-13-6-dep-9-3-armv7.cmake
	новый файл:    tools/polly/ios-13-6-dep-9-3-armv7s.cmake
	новый файл:    tools/polly/ios-13-6-dep-9-3-device-bitcode-cxx14.cmake
	новый файл:    tools/polly/ios-13-6-dep-9-3-device-cxx14.cmake
	новый файл:    tools/polly/ios-14-0-dep-9-3-arm64.cmake
	новый файл:    tools/polly/ios-14-0-dep-9-3-armv7.cmake
	новый файл:    tools/polly/ios-14-0-dep-9-3-armv7s.cmake
	новый файл:    tools/polly/ios-14-0-dep-9-3-device-bitcode-cxx14.cmake
	новый файл:    tools/polly/ios-14-0-dep-9-3-device-cxx14.cmake
	новый файл:    tools/polly/ios-14-2-dep-10-0-arm64.cmake
	новый файл:    tools/polly/ios-14-2-dep-10-0-armv7.cmake
	новый файл:    tools/polly/ios-14-2-dep-10-0-armv7s.cmake
	новый файл:    tools/polly/ios-14-2-dep-10-0-device-bitcode-cxx14.cmake
	новый файл:    tools/polly/ios-14-2-dep-10-0-device-cxx14.cmake
	новый файл:    tools/polly/ios-14-3-dep-10-0-arm64.cmake
	новый файл:    tools/polly/ios-14-3-dep-10-0-armv7.cmake
	новый файл:    tools/polly/ios-14-3-dep-10-0-armv7s.cmake
	новый файл:    tools/polly/ios-14-3-dep-10-0-device-bitcode-cxx14.cmake
	новый файл:    tools/polly/ios-14-3-dep-10-0-device-cxx14.cmake
	новый файл:    tools/polly/ios-14-4-dep-10-0-arm64.cmake
	новый файл:    tools/polly/ios-14-4-dep-10-0-armv7.cmake
	новый файл:    tools/polly/ios-14-4-dep-10-0-armv7s.cmake
	новый файл:    tools/polly/ios-14-4-dep-10-0-device-bitcode-cxx14.cmake
	новый файл:    tools/polly/ios-14-4-dep-10-0-device-cxx14.cmake
	новый файл:    tools/polly/ios-7-0.cmake
	новый файл:    tools/polly/ios-7-1.cmake
	новый файл:    tools/polly/ios-8-0.cmake
	новый файл:    tools/polly/ios-8-1.cmake
	новый файл:    tools/polly/ios-8-2-arm64-hid.cmake
	новый файл:    tools/polly/ios-8-2-arm64.cmake
	новый файл:    tools/polly/ios-8-2-cxx98.cmake
	новый файл:    tools/polly/ios-8-2-i386-arm64.cmake
	новый файл:    tools/polly/ios-8-2.cmake
	новый файл:    tools/polly/ios-8-4-arm64.cmake
	новый файл:    tools/polly/ios-8-4-armv7.cmake
	новый файл:    tools/polly/ios-8-4-armv7s.cmake
	новый файл:    tools/polly/ios-8-4-hid.cmake
	новый файл:    tools/polly/ios-8-4.cmake
	новый файл:    tools/polly/ios-9-0-armv7.cmake
	новый файл:    tools/polly/ios-9-0-dep-7-0-armv7.cmake
	новый файл:    tools/polly/ios-9-0-i386-armv7.cmake
	новый файл:    tools/polly/ios-9-0-wo-armv7s.cmake
	новый файл:    tools/polly/ios-9-0.cmake
	новый файл:    tools/polly/ios-9-1-arm64.cmake
	новый файл:    tools/polly/ios-9-1-armv7.cmake
	новый файл:    tools/polly/ios-9-1-dep-7-0-armv7.cmake
	новый файл:    tools/polly/ios-9-1-dep-8-0-hid.cmake
	новый файл:    tools/polly/ios-9-1-hid.cmake
	новый файл:    tools/polly/ios-9-1.cmake
	новый файл:    tools/polly/ios-9-2-arm64.cmake
	новый файл:    tools/polly/ios-9-2-armv7.cmake
	новый файл:    tools/polly/ios-9-2-hid-sections.cmake
	новый файл:    tools/polly/ios-9-2-hid.cmake
	новый файл:    tools/polly/ios-9-2.cmake
	новый файл:    tools/polly/ios-9-3-arm64.cmake
	новый файл:    tools/polly/ios-9-3-armv7.cmake
	новый файл:    tools/polly/ios-9-3-wo-armv7s.cmake
	новый файл:    tools/polly/ios-9-3.cmake
	новый файл:    tools/polly/ios-bitcode.cmake
	новый файл:    tools/polly/ios-cxx17.cmake
	новый файл:    tools/polly/ios-dep-10-0-bitcode-cxx17.cmake
	новый файл:    tools/polly/ios-dep-11-0-bitcode-cxx17.cmake
	новый файл:    tools/polly/ios-dep-12-0-bitcode-cxx17.cmake
	новый файл:    tools/polly/ios-dep-8-0-arm64-armv7-hid-sections-cxx11.cmake
	новый файл:    tools/polly/ios-dep-8-0-arm64-armv7-hid-sections-lto-cxx11.cmake
	новый файл:    tools/polly/ios-dep-8-0-arm64-cxx11.cmake
	новый файл:    tools/polly/ios-nocodesign-10-0-arm64.cmake
	новый файл:    tools/polly/ios-nocodesign-10-0-armv7.cmake
	новый файл:    tools/polly/ios-nocodesign-10-0-wo-armv7s.cmake
	новый файл:    tools/polly/ios-nocodesign-10-0.cmake
	новый файл:    tools/polly/ios-nocodesign-10-1-arm64-dep-9-0-device-libcxx-hid-sections-lto.cmake
	новый файл:    tools/polly/ios-nocodesign-10-1-arm64-dep-9-0-device-libcxx-hid-sections.cmake
	новый файл:    tools/polly/ios-nocodesign-10-1-arm64.cmake
	новый файл:    tools/polly/ios-nocodesign-10-1-armv7.cmake
	новый файл:    tools/polly/ios-nocodesign-10-1-dep-8-0-device-libcxx-hid-sections-lto.cmake
	новый файл:    tools/polly/ios-nocodesign-10-1-dep-8-0-libcxx-hid-sections-lto.cmake
	новый файл:    tools/polly/ios-nocodesign-10-1-dep-9-0-device-libcxx-hid-sections-lto.cmake
	новый файл:    tools/polly/ios-nocodesign-10-1-wo-armv7s.cmake
	новый файл:    tools/polly/ios-nocodesign-10-1.cmake
	новый файл:    tools/polly/ios-nocodesign-10-2.cmake
	новый файл:    tools/polly/ios-nocodesign-10-3-arm64-dep-9-0-device-libcxx-hid-sections.cmake
	новый файл:    tools/polly/ios-nocodesign-10-3-arm64.cmake
	новый файл:    tools/polly/ios-nocodesign-10-3-armv7.cmake
	новый файл:    tools/polly/ios-nocodesign-10-3-cxx14.cmake
	новый файл:    tools/polly/ios-nocodesign-10-3-dep-9-0-bitcode.cmake
	новый файл:    tools/polly/ios-nocodesign-10-3-wo-armv7s.cmake
	новый файл:    tools/polly/ios-nocodesign-10-3.cmake
	новый файл:    tools/polly/ios-nocodesign-11-0-arm64-dep-9-0-device-libcxx-hid-sections.cmake
	новый файл:    tools/polly/ios-nocodesign-11-0-dep-9-0-bitcode-cxx11.cmake
	новый файл:    tools/polly/ios-nocodesign-11-0.cmake
	новый файл:    tools/polly/ios-nocodesign-11-1-dep-9-0-bitcode-cxx11.cmake
	новый файл:    tools/polly/ios-nocodesign-11-1-dep-9-0-wo-armv7s-bitcode-cxx11.cmake
	новый файл:    tools/polly/ios-nocodesign-11-1.cmake
	новый файл:    tools/polly/ios-nocodesign-11-2-dep-8-0-wo-armv7s-bitcode-cxx11.cmake
	новый файл:    tools/polly/ios-nocodesign-11-2-dep-9-0-bitcode-cxx11.cmake
	новый файл:    tools/polly/ios-nocodesign-11-2-dep-9-3-arm64-armv7.cmake
	новый файл:    tools/polly/ios-nocodesign-11-2-dep-9-3-arm64.cmake
	новый файл:    tools/polly/ios-nocodesign-11-2-dep-9-3-armv7.cmake
	новый файл:    tools/polly/ios-nocodesign-11-2-dep-9-3-i386-armv7.cmake
	новый файл:    tools/polly/ios-nocodesign-11-2-dep-9-3.cmake
	новый файл:    tools/polly/ios-nocodesign-11-2.cmake
	новый файл:    tools/polly/ios-nocodesign-11-3-dep-9-0-bitcode-cxx11.cmake
	новый файл:    tools/polly/ios-nocodesign-11-3-dep-9-3-arm64.cmake
	новый файл:    tools/polly/ios-nocodesign-11-3-dep-9-3-armv7.cmake
	новый файл:    tools/polly/ios-nocodesign-11-3-dep-9-3.cmake
	новый файл:    tools/polly/ios-nocodesign-11-4-dep-9-0-bitcode-cxx11.cmake
	новый файл:    tools/polly/ios-nocodesign-11-4-dep-9-3-arm64.cmake
	новый файл:    tools/polly/ios-nocodesign-11-4-dep-9-3-armv7.cmake
	новый файл:    tools/polly/ios-nocodesign-11-4-dep-9-3.cmake
	новый файл:    tools/polly/ios-nocodesign-12-0-dep-10-0-bitcode-cxx11.cmake
	новый файл:    tools/polly/ios-nocodesign-12-0-dep-9-0-bitcode-cxx11.cmake
	новый файл:    tools/polly/ios-nocodesign-12-1-dep-9-0-bitcode-cxx11.cmake
	новый файл:    tools/polly/ios-nocodesign-12-1-dep-9-3-armv7.cmake
	новый файл:    tools/polly/ios-nocodesign-12-1.cmake
	новый файл:    tools/polly/ios-nocodesign-13-0-dep-9-3-arm64.cmake
	новый файл:    tools/polly/ios-nocodesign-13-2-dep-9-3-arm64.cmake
	новый файл:    tools/polly/ios-nocodesign-13-2-dep-9-3-armv7.cmake
	новый файл:    tools/polly/ios-nocodesign-13-2-dep-9-3-armv7s.cmake
	новый файл:    tools/polly/ios-nocodesign-13-2-dep-9-3-device-cxx11.cmake
	новый файл:    tools/polly/ios-nocodesign-13-2-dep-9-3-device.cmake
	новый файл:    tools/polly/ios-nocodesign-13-2-dep-9-3.cmake
	новый файл:    tools/polly/ios-nocodesign-13-5-dep-9-3-arm64.cmake
	новый файл:    tools/polly/ios-nocodesign-13-5-dep-9-3-armv7.cmake
	новый файл:    tools/polly/ios-nocodesign-13-5-dep-9-3-armv7s.cmake
	новый файл:    tools/polly/ios-nocodesign-13-5-dep-9-3-device-cxx11.cmake
	новый файл:    tools/polly/ios-nocodesign-13-5-dep-9-3-device.cmake
	новый файл:    tools/polly/ios-nocodesign-13-5-dep-9-3.cmake
	новый файл:    tools/polly/ios-nocodesign-13-6-dep-9-3-arm64.cmake
	новый файл:    tools/polly/ios-nocodesign-13-6-dep-9-3-armv7.cmake
	новый файл:    tools/polly/ios-nocodesign-13-6-dep-9-3-armv7s.cmake
	новый файл:    tools/polly/ios-nocodesign-13-6-dep-9-3-device-cxx11.cmake
	новый файл:    tools/polly/ios-nocodesign-13-6-dep-9-3-device.cmake
	новый файл:    tools/polly/ios-nocodesign-13-6-dep-9-3.cmake
	новый файл:    tools/polly/ios-nocodesign-14-0-dep-9-3-arm64.cmake
	новый файл:    tools/polly/ios-nocodesign-14-0-dep-9-3-armv7.cmake
	новый файл:    tools/polly/ios-nocodesign-14-0-dep-9-3-armv7s.cmake
	новый файл:    tools/polly/ios-nocodesign-14-0-dep-9-3-device-cxx11.cmake
	новый файл:    tools/polly/ios-nocodesign-14-0-dep-9-3-device.cmake
	новый файл:    tools/polly/ios-nocodesign-14-0-dep-9-3.cmake
	новый файл:    tools/polly/ios-nocodesign-14-2-dep-10-0-arm64.cmake
	новый файл:    tools/polly/ios-nocodesign-14-2-dep-10-0-armv7.cmake
	новый файл:    tools/polly/ios-nocodesign-14-2-dep-10-0-armv7s.cmake
	новый файл:    tools/polly/ios-nocodesign-14-2-dep-10-0-device-cxx11.cmake
	новый файл:    tools/polly/ios-nocodesign-14-2-dep-10-0-device.cmake
	новый файл:    tools/polly/ios-nocodesign-14-2-dep-10-0.cmake
	новый файл:    tools/polly/ios-nocodesign-14-3-dep-10-0-arm64.cmake
	новый файл:    tools/polly/ios-nocodesign-14-3-dep-10-0-armv7.cmake
	новый файл:    tools/polly/ios-nocodesign-14-3-dep-10-0-armv7s.cmake
	новый файл:    tools/polly/ios-nocodesign-14-3-dep-10-0-device-cxx11.cmake
	новый файл:    tools/polly/ios-nocodesign-14-3-dep-10-0-device.cmake
	новый файл:    tools/polly/ios-nocodesign-14-3-dep-10-0.cmake
	новый файл:    tools/polly/ios-nocodesign-14-4-dep-10-0-arm64.cmake
	новый файл:    tools/polly/ios-nocodesign-14-4-dep-10-0-armv7.cmake
	новый файл:    tools/polly/ios-nocodesign-14-4-dep-10-0-armv7s.cmake
	новый файл:    tools/polly/ios-nocodesign-14-4-dep-10-0-device-cxx11.cmake
	новый файл:    tools/polly/ios-nocodesign-14-4-dep-10-0-device.cmake
	новый файл:    tools/polly/ios-nocodesign-14-4-dep-10-0.cmake
	новый файл:    tools/polly/ios-nocodesign-8-1.cmake
	новый файл:    tools/polly/ios-nocodesign-8-4.cmake
	новый файл:    tools/polly/ios-nocodesign-9-1-arm64.cmake
	новый файл:    tools/polly/ios-nocodesign-9-1-armv7.cmake
	новый файл:    tools/polly/ios-nocodesign-9-1.cmake
	новый файл:    tools/polly/ios-nocodesign-9-2-arm64.cmake
	новый файл:    tools/polly/ios-nocodesign-9-2-armv7.cmake
	новый файл:    tools/polly/ios-nocodesign-9-2.cmake
	новый файл:    tools/polly/ios-nocodesign-9-3-arm64.cmake
	новый файл:    tools/polly/ios-nocodesign-9-3-armv7.cmake
	новый файл:    tools/polly/ios-nocodesign-9-3-device-hid-sections.cmake
	новый файл:    tools/polly/ios-nocodesign-9-3-device.cmake
	новый файл:    tools/polly/ios-nocodesign-9-3-wo-armv7s.cmake
	новый файл:    tools/polly/ios-nocodesign-9-3.cmake
	новый файл:    tools/polly/ios-nocodesign-arm64.cmake
	новый файл:    tools/polly/ios-nocodesign-armv7.cmake
	новый файл:    tools/polly/ios-nocodesign-dep-9-0-cxx14.cmake
	новый файл:    tools/polly/ios-nocodesign-hid-sections.cmake
	новый файл:    tools/polly/ios-nocodesign-wo-armv7s.cmake
	новый файл:    tools/polly/ios-nocodesign.cmake
	новый файл:    tools/polly/ios.cmake
	новый файл:    tools/polly/libcxx-fpic-hid-sections.cmake
	новый файл:    tools/polly/libcxx-hid-fpic.cmake
	новый файл:    tools/polly/libcxx-hid-sections.cmake
	новый файл:    tools/polly/libcxx-hid.cmake
	новый файл:    tools/polly/libcxx-no-sdk.cmake
	новый файл:    tools/polly/libcxx.cmake
	новый файл:    tools/polly/libcxx14.cmake
	новый файл:    tools/polly/library/std/libcxx.cmake
	новый файл:    tools/polly/library/std/libstdcxx.cmake
	новый файл:    tools/polly/library/std/nolibs.cmake
	новый файл:    tools/polly/linux-gcc-armhf-neon-vfpv4.cmake
	новый файл:    tools/polly/linux-gcc-armhf-neon.cmake
	новый файл:    tools/polly/linux-gcc-armhf.cmake
	новый файл:    tools/polly/linux-gcc-jetson-tk1.cmake
	новый файл:    tools/polly/linux-gcc-x64.cmake
	новый файл:    tools/polly/linux-mingw-w32-cxx14.cmake
	новый файл:    tools/polly/linux-mingw-w32.cmake
	новый файл:    tools/polly/linux-mingw-w64-cxx14.cmake
	новый файл:    tools/polly/linux-mingw-w64-cxx98.cmake
	новый файл:    tools/polly/linux-mingw-w64-gnuxx11.cmake
	новый файл:    tools/polly/linux-mingw-w64.cmake
	новый файл:    tools/polly/mingw-c11.cmake
	новый файл:    tools/polly/mingw-cxx14.cmake
	новый файл:    tools/polly/mingw-cxx17.cmake
	новый файл:    tools/polly/mingw.cmake
	новый файл:    tools/polly/msys-cxx14.cmake
	новый файл:    tools/polly/msys-cxx17.cmake
	новый файл:    tools/polly/msys.cmake
	новый файл:    tools/polly/ninja-gcc-7-cxx17-concepts.cmake
	новый файл:    tools/polly/ninja-gcc-8-cxx17-concepts.cmake
	новый файл:    tools/polly/ninja-vs-12-2013-win64.cmake
	новый файл:    tools/polly/ninja-vs-14-2015-win64.cmake
	новый файл:    tools/polly/ninja-vs-15-2017-win64-cxx17-nonpermissive.cmake
	новый файл:    tools/polly/ninja-vs-15-2017-win64-cxx17.cmake
	новый файл:    tools/polly/ninja-vs-15-2017-win64.cmake
	новый файл:    tools/polly/nmake-vs-12-2013-win64.cmake
	новый файл:    tools/polly/nmake-vs-12-2013.cmake
	новый файл:    tools/polly/nmake-vs-15-2017-win64-cxx17-nonpermissive.cmake
	новый файл:    tools/polly/nmake-vs-15-2017-win64-cxx17.cmake
	новый файл:    tools/polly/nmake-vs-15-2017-win64.cmake
	новый файл:    tools/polly/openbsd-egcc-cxx11-static-std.cmake
	новый файл:    tools/polly/os/android.cmake
	новый файл:    tools/polly/os/cygwin.cmake
	новый файл:    tools/polly/os/iphone-default-sdk.cmake
	новый файл:    tools/polly/os/iphone.cmake
	новый файл:    tools/polly/os/osx.cmake
	новый файл:    tools/polly/os/raspberry-pi-hardfloat.cmake
	новый файл:    tools/polly/os/raspberry-pi1.cmake
	новый файл:    tools/polly/os/raspberry-pi2.cmake
	новый файл:    tools/polly/os/raspberry-pi3.cmake
	новый файл:    tools/polly/os/rpi-sysroot.cmake
	новый файл:    tools/polly/os/vc-mdd-android.cmake
	новый файл:    tools/polly/osx-10-10-dep-10-7.cmake
	новый файл:    tools/polly/osx-10-10-dep-10-9-make.cmake
	новый файл:    tools/polly/osx-10-10.cmake
	новый файл:    tools/polly/osx-10-11-hid-sections-lto.cmake
	новый файл:    tools/polly/osx-10-11-hid-sections.cmake
	новый файл:    tools/polly/osx-10-11-lto.cmake
	новый файл:    tools/polly/osx-10-11-make.cmake
	новый файл:    tools/polly/osx-10-11-sanitize-address.cmake
	новый файл:    tools/polly/osx-10-11.cmake
	новый файл:    tools/polly/osx-10-12-cxx14.cmake
	новый файл:    tools/polly/osx-10-12-cxx17.cmake
	новый файл:    tools/polly/osx-10-12-cxx98.cmake
	новый файл:    tools/polly/osx-10-12-dep-10-10-lto.cmake
	новый файл:    tools/polly/osx-10-12-dep-10-10.cmake
	новый файл:    tools/polly/osx-10-12-hid-sections.cmake
	новый файл:    tools/polly/osx-10-12-lto.cmake
	новый файл:    tools/polly/osx-10-12-make.cmake
	новый файл:    tools/polly/osx-10-12-ninja.cmake
	новый файл:    tools/polly/osx-10-12-sanitize-address-hid-sections.cmake
	новый файл:    tools/polly/osx-10-12-sanitize-address.cmake
	новый файл:    tools/polly/osx-10-12.cmake
	новый файл:    tools/polly/osx-10-13-cxx14.cmake
	новый файл:    tools/polly/osx-10-13-cxx17.cmake
	новый файл:    tools/polly/osx-10-13-dep-10-10-cxx14.cmake
	новый файл:    tools/polly/osx-10-13-dep-10-10-cxx17.cmake
	новый файл:    tools/polly/osx-10-13-dep-10-10.cmake
	новый файл:    tools/polly/osx-10-13-dep-10-12-cxx14.cmake
	новый файл:    tools/polly/osx-10-13-dep-10-12-cxx17.cmake
	новый файл:    tools/polly/osx-10-13-dep-10-12.cmake
	новый файл:    tools/polly/osx-10-13-i386-cxx14.cmake
	новый файл:    tools/polly/osx-10-13-make-cxx14.cmake
	новый файл:    tools/polly/osx-10-13.cmake
	новый файл:    tools/polly/osx-10-14-cxx14.cmake
	новый файл:    tools/polly/osx-10-14-cxx17.cmake
	новый файл:    tools/polly/osx-10-14-dep-10-10-cxx14.cmake
	новый файл:    tools/polly/osx-10-14-dep-10-10-cxx17.cmake
	новый файл:    tools/polly/osx-10-14-dep-10-10.cmake
	новый файл:    tools/polly/osx-10-14-dep-10-12-cxx14.cmake
	новый файл:    tools/polly/osx-10-14-dep-10-12-cxx17.cmake
	новый файл:    tools/polly/osx-10-14-dep-10-12.cmake
	новый файл:    tools/polly/osx-10-14.cmake
	новый файл:    tools/polly/osx-10-15-cxx17.cmake
	новый файл:    tools/polly/osx-10-15-dep-10-10-cxx14.cmake
	новый файл:    tools/polly/osx-10-15-dep-10-10-cxx17.cmake
	новый файл:    tools/polly/osx-10-15-dep-10-10.cmake
	новый файл:    tools/polly/osx-10-15-dep-10-12-cxx17.cmake
	новый файл:    tools/polly/osx-10-15.cmake
	новый файл:    tools/polly/osx-10-7.cmake
	новый файл:    tools/polly/osx-10-8.cmake
	новый файл:    tools/polly/osx-10-9.cmake
	новый файл:    tools/polly/osx-11-0-cxx17.cmake
	новый файл:    tools/polly/osx-11-0-dep-10-10-cxx17.cmake
	новый файл:    tools/polly/osx-11-0.cmake
	новый файл:    tools/polly/osx-11-1-cxx17.cmake
	новый файл:    tools/polly/osx-11-1-dep-10-10-cxx17.cmake
	новый файл:    tools/polly/osx-11-1.cmake
	новый файл:    tools/polly/raspberrypi1-cxx11-pic-static-std.cmake
	новый файл:    tools/polly/raspberrypi1-cxx11-pic.cmake
	новый файл:    tools/polly/raspberrypi1-cxx14-pic-static-std.cmake
	новый файл:    tools/polly/raspberrypi2-cxx11-pic.cmake
	новый файл:    tools/polly/raspberrypi2-cxx11.cmake
	новый файл:    tools/polly/raspberrypi3-clang-cxx11.cmake
	новый файл:    tools/polly/raspberrypi3-clang-cxx14-pic.cmake
	новый файл:    tools/polly/raspberrypi3-clang-cxx14.cmake
	новый файл:    tools/polly/raspberrypi3-cxx11.cmake
	новый файл:    tools/polly/raspberrypi3-cxx14.cmake
	новый файл:    tools/polly/raspberrypi3-gcc-pic-hid-sections.cmake
	новый файл:    tools/polly/sanitize-address-cxx17-pic.cmake
	новый файл:    tools/polly/sanitize-address-cxx17.cmake
	новый файл:    tools/polly/sanitize-address.cmake
	новый файл:    tools/polly/sanitize-leak-cxx17-pic.cmake
	новый файл:    tools/polly/sanitize-leak-cxx17.cmake
	новый файл:    tools/polly/sanitize-leak.cmake
	новый файл:    tools/polly/sanitize-memory.cmake
	новый файл:    tools/polly/sanitize-thread-cxx17-pic.cmake
	новый файл:    tools/polly/sanitize-thread-cxx17.cmake
	новый файл:    tools/polly/sanitize-thread.cmake
	новый файл:    tools/polly/scripts/Info.plist
	новый файл:    tools/polly/scripts/NoCodeSign.xcconfig
	новый файл:    tools/polly/scripts/clang-analyze.sh
	новый файл:    tools/polly/scripts/clangxx-analyze.sh
	новый файл:    tools/polly/utilities/polly_add_cache_flag.cmake
	новый файл:    tools/polly/utilities/polly_clear_environment_variables.cmake
	новый файл:    tools/polly/utilities/polly_common.cmake
	новый файл:    tools/polly/utilities/polly_fatal_error.cmake
	новый файл:    tools/polly/utilities/polly_init.cmake
	новый файл:    tools/polly/utilities/polly_ios_bundle_identifier.cmake
	новый файл:    tools/polly/utilities/polly_ios_development_team.cmake
	новый файл:    tools/polly/utilities/polly_module_path.cmake
	новый файл:    tools/polly/utilities/polly_status_debug.cmake
	новый файл:    tools/polly/utilities/polly_status_print.cmake
	новый файл:    tools/polly/vs-10-2010.cmake
	новый файл:    tools/polly/vs-11-2012-arm.cmake
	новый файл:    tools/polly/vs-11-2012-win64.cmake
	новый файл:    tools/polly/vs-11-2012.cmake
	новый файл:    tools/polly/vs-12-2013-arm.cmake
	новый файл:    tools/polly/vs-12-2013-mt.cmake
	новый файл:    tools/polly/vs-12-2013-win64.cmake
	новый файл:    tools/polly/vs-12-2013-xp.cmake
	новый файл:    tools/polly/vs-12-2013.cmake
	новый файл:    tools/polly/vs-14-2015-arm.cmake
	новый файл:    tools/polly/vs-14-2015-sdk-8-1.cmake
	новый файл:    tools/polly/vs-14-2015-win64-sdk-8-1.cmake
	новый файл:    tools/polly/vs-14-2015-win64.cmake
	новый файл:    tools/polly/vs-14-2015.cmake
	новый файл:    tools/polly/vs-15-2017-cxx14-mt.cmake
	новый файл:    tools/polly/vs-15-2017-cxx17.cmake
	новый файл:    tools/polly/vs-15-2017-mt.cmake
	новый файл:    tools/polly/vs-15-2017-store-10-zw.cmake
	новый файл:    tools/polly/vs-15-2017-win64-cxx14-mt.cmake
	новый файл:    tools/polly/vs-15-2017-win64-cxx14.cmake
	новый файл:    tools/polly/vs-15-2017-win64-cxx17-nonpermissive.cmake
	новый файл:    tools/polly/vs-15-2017-win64-cxx17.cmake
	новый файл:    tools/polly/vs-15-2017-win64-llvm-vs2014.cmake
	новый файл:    tools/polly/vs-15-2017-win64-llvm.cmake
	новый файл:    tools/polly/vs-15-2017-win64-mt.cmake
	новый файл:    tools/polly/vs-15-2017-win64-store-10-cxx17.cmake
	новый файл:    tools/polly/vs-15-2017-win64-store-10-zw.cmake
	новый файл:    tools/polly/vs-15-2017-win64-version-14-11.cmake
	новый файл:    tools/polly/vs-15-2017-win64-z7.cmake
	новый файл:    tools/polly/vs-15-2017-win64.cmake
	новый файл:    tools/polly/vs-15-2017.cmake
	новый файл:    tools/polly/vs-16-2019-cxx14.cmake
	новый файл:    tools/polly/vs-16-2019-cxx17.cmake
	новый файл:    tools/polly/vs-16-2019-llvm-cxx17.cmake
	новый файл:    tools/polly/vs-16-2019-win64-cxx14.cmake
	новый файл:    tools/polly/vs-16-2019-win64-cxx17.cmake
	новый файл:    tools/polly/vs-16-2019-win64-llvm-cxx17.cmake
	новый файл:    tools/polly/vs-16-2019-win64.cmake
	новый файл:    tools/polly/vs-16-2019.cmake
	новый файл:    tools/polly/vs-8-2005.cmake
	новый файл:    tools/polly/vs-9-2008.cmake
	новый файл:    tools/polly/xcode-cxx17.cmake
	новый файл:    tools/polly/xcode-cxx98.cmake
	новый файл:    tools/polly/xcode-gcc.cmake
	новый файл:    tools/polly/xcode-hid-sections.cmake
	новый файл:    tools/polly/xcode-nocxx.cmake
	новый файл:    tools/polly/xcode-sections.cmake
	новый файл:    tools/polly/xcode.cmake

 ~/ Kinpach/workspace/TimpLab08$ git commit -m "adding Dockerfile"
[master a88a687] adding Dockerfile
 810 files changed, 31279 insertions(+), 86 deletions(-)
 create mode 100644 cmake/Hunter/config.cmake
 create mode 100644 cmake/HunterGate.cmake
 create mode 100644 demo/main.cpp
 create mode 100644 logs/log.txt
 create mode 100644 tools/polly/.gitignore
 create mode 100644 tools/polly/.gitmodules
 create mode 100644 tools/polly/.travis.yml
 create mode 100644 tools/polly/CONTRIBUTING.md
 create mode 100644 tools/polly/LICENSE
 create mode 100644 tools/polly/analyze-cxx17.cmake
 create mode 100644 tools/polly/analyze.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-16-armeabi-v7a-neon-clang-35-hid-sections.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-16-armeabi-v7a-neon-clang-35-hid.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-16-armeabi-v7a-neon-clang-35.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-16-armeabi-v7a-neon.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-16-x86-hid-sections.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-16-x86-hid.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-16-x86.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-19-armeabi-v7a-neon-c11.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-19-armeabi-v7a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-19-armeabi-v7a-neon-hid-sections-lto.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-19-armeabi-v7a-neon-hid-sections.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-19-armeabi-v7a-neon.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-arm64-v8a-clang-35.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-arm64-v8a-gcc-49-hid-sections.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-arm64-v8a-gcc-49-hid.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-arm64-v8a-gcc-49.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-arm64-v8a.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-armeabi-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-armeabi-v7a-neon-clang-35.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-armeabi-v7a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-armeabi-v7a-neon-hid-sections.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-armeabi-v7a-neon.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-armeabi-v7a.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-armeabi.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-mips-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-mips.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-mips64.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-x86-64-hid-sections.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-x86-64-hid.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-x86-64.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-x86-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-x86.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-8-armeabi-v7a.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-16-armeabi-cxx14.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-16-armeabi-v7a-cxx14.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-16-armeabi-v7a-neon-clang-35-hid.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-16-armeabi-v7a-neon-clang-35.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-16-armeabi-v7a-neon-cxx14.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-16-armeabi-v7a-neon.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-16-armeabi-v7a.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-16-armeabi.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-16-x86-hid.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-16-x86.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-19-armeabi-v7a-neon.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-arm64-v8a-clang-35.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-arm64-v8a-gcc-49-hid.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-arm64-v8a-gcc-49.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-arm64-v8a.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-armeabi-v7a-neon-clang-35.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-armeabi-v7a-neon.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-armeabi-v7a.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-armeabi.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-mips.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-mips64.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-x86-64-hid.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-x86-64.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-x86.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-8-armeabi-v7a.cmake
 create mode 100644 tools/polly/android-ndk-r12b-api-19-armeabi-v7a-neon.cmake
 create mode 100644 tools/polly/android-ndk-r13b-api-19-armeabi-v7a-neon.cmake
 create mode 100644 tools/polly/android-ndk-r14-api-16-armeabi-v7a-neon-clang-hid-sections-lto.cmake
 create mode 100644 tools/polly/android-ndk-r14-api-19-armeabi-v7a-neon-c11.cmake
 create mode 100644 tools/polly/android-ndk-r14-api-19-armeabi-v7a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r14-api-19-armeabi-v7a-neon-clang.cmake
 create mode 100644 tools/polly/android-ndk-r14-api-19-armeabi-v7a-neon-hid-sections-lto.cmake
 create mode 100644 tools/polly/android-ndk-r14-api-21-arm64-v8a-clang-hid-sections-lto.cmake
 create mode 100644 tools/polly/android-ndk-r14-api-21-arm64-v8a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r14-api-21-x86-64.cmake
 create mode 100644 tools/polly/android-ndk-r14b-api-21-armeabi-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r14b-api-21-armeabi-v7a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r14b-api-21-mips-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r14b-api-21-x86-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-16-armeabi-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-16-armeabi-v7a-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-16-armeabi-v7a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-16-mips-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-16-x86-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-21-arm64-v8a-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-21-arm64-v8a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-21-armeabi-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-21-armeabi-v7a-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-21-armeabi-v7a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-21-mips-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-21-x86-64-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-21-x86-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-24-armeabi-v7a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-16-armeabi-v7a-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-16-armeabi-v7a-thumb-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-16-x86-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-19-gcc-49-armeabi-v7a-neon-libcxx-hid-sections-lto.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-21-arm64-v8a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-21-arm64-v8a-neon-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-21-armeabi-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-21-armeabi-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-21-armeabi-v7a-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-21-armeabi-v7a-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-21-armeabi-v7a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-21-armeabi-v7a-neon-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-21-x86-64-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-21-x86-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-24-arm64-v8a-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-24-armeabi-v7a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-24-armeabi-v7a-neon-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-24-x86-64-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-24-x86-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r17-api-16-armeabi-v7a-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r17-api-16-x86-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r17-api-19-armeabi-v7a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r17-api-19-armeabi-v7a-neon-hid-sections.cmake
 create mode 100644 tools/polly/android-ndk-r17-api-21-arm64-v8a-neon-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r17-api-21-x86-64-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r17-api-24-arm64-v8a-clang-libcxx11.cmake
 create mode 100644 tools/polly/android-ndk-r17-api-24-arm64-v8a-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r18-api-24-arm64-v8a-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r18b-api-16-armeabi-v7a-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r18b-api-21-arm64-v8a-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r18b-api-21-armeabi-v7a-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r18b-api-21-x86-64-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r18b-api-21-x86-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r18b-api-24-arm64-v8a-clang-libcxx11.cmake
 create mode 100644 tools/polly/android-ndk-r18b-api-28-arm64-v8a-clang-libcxx11.cmake
 create mode 100644 tools/polly/android-vc-ndk-r10e-api-19-arm-clang-3-6.cmake
 create mode 100644 tools/polly/android-vc-ndk-r10e-api-19-arm-gcc-4-9.cmake
 create mode 100644 tools/polly/android-vc-ndk-r10e-api-19-x86-clang-3-6.cmake
 create mode 100644 tools/polly/android-vc-ndk-r10e-api-21-arm-clang-3-6.cmake
 create mode 100644 tools/polly/appveyor.yml
 create mode 100644 tools/polly/arm-openwrt-linux-muslgnueabi-cxx14.cmake
 create mode 100644 tools/polly/arm-openwrt-linux-muslgnueabi.cmake
 create mode 100755 tools/polly/bin/build.py
 create mode 100644 tools/polly/bin/detail/__init__.py
 create mode 100644 tools/polly/bin/detail/call.py
 create mode 100644 tools/polly/bin/detail/cpack_generator.py
 create mode 100644 tools/polly/bin/detail/create_archive.py
 create mode 100644 tools/polly/bin/detail/create_framework.py
 create mode 100644 tools/polly/bin/detail/generate_command.py
 create mode 100755 tools/polly/bin/detail/get_nmake_environment.py
 create mode 100644 tools/polly/bin/detail/ios_dev_root.py
 create mode 100644 tools/polly/bin/detail/logging.py
 create mode 100644 tools/polly/bin/detail/open_project.py
 create mode 100644 tools/polly/bin/detail/osx_dev_root.py
 create mode 100644 tools/polly/bin/detail/pack_command.py
 create mode 100644 tools/polly/bin/detail/rmtree.py
 create mode 100644 tools/polly/bin/detail/target.py
 create mode 100644 tools/polly/bin/detail/test_command.py
 create mode 100644 tools/polly/bin/detail/timer.py
 create mode 100644 tools/polly/bin/detail/toolchain_name.py
 create mode 100644 tools/polly/bin/detail/toolchain_table.py
 create mode 100755 tools/polly/bin/detail/util.py
 create mode 100644 tools/polly/bin/detail/verify_mingw_path.py
 create mode 100644 tools/polly/bin/detail/verify_msys_path.py
 create mode 100755 tools/polly/bin/detail/win32.py
 create mode 100755 tools/polly/bin/install-ci-dependencies.py
 create mode 100755 tools/polly/bin/polly
 create mode 100644 tools/polly/bin/polly.bat
 create mode 100755 tools/polly/bin/polly.py
 create mode 100644 tools/polly/clang-5-cxx14.cmake
 create mode 100644 tools/polly/clang-5-cxx17.cmake
 create mode 100644 tools/polly/clang-5.cmake
 create mode 100644 tools/polly/clang-cxx11.cmake
 create mode 100644 tools/polly/clang-cxx14-pic.cmake
 create mode 100644 tools/polly/clang-cxx14.cmake
 create mode 100644 tools/polly/clang-cxx17-pic.cmake
 create mode 100644 tools/polly/clang-cxx17.cmake
 create mode 100644 tools/polly/clang-cxx20.cmake
 create mode 100644 tools/polly/clang-fpic-hid-sections.cmake
 create mode 100644 tools/polly/clang-fpic-static-std-cxx14.cmake
 create mode 100644 tools/polly/clang-fpic-static-std.cmake
 create mode 100644 tools/polly/clang-fpic.cmake
 create mode 100644 tools/polly/clang-libcxx-fpic.cmake
 create mode 100644 tools/polly/clang-libcxx.cmake
 create mode 100644 tools/polly/clang-libcxx14-fpic.cmake
 create mode 100644 tools/polly/clang-libcxx14.cmake
 create mode 100644 tools/polly/clang-libcxx17-fpic.cmake
 create mode 100644 tools/polly/clang-libcxx17-static.cmake
 create mode 100644 tools/polly/clang-libcxx17.cmake
 create mode 100644 tools/polly/clang-libcxx98.cmake
 create mode 100644 tools/polly/clang-libstdcxx.cmake
 create mode 100644 tools/polly/clang-lto.cmake
 create mode 100644 tools/polly/clang-omp.cmake
 create mode 100644 tools/polly/clang-tidy-libcxx.cmake
 create mode 100644 tools/polly/clang-tidy.cmake
 create mode 100644 tools/polly/compiler/cl.cmake
 create mode 100644 tools/polly/compiler/clang-5.cmake
 create mode 100644 tools/polly/compiler/clang-omp.cmake
 create mode 100644 tools/polly/compiler/clang-tools.cmake
 create mode 100644 tools/polly/compiler/clang.cmake
 create mode 100644 tools/polly/compiler/egcc.cmake
 create mode 100644 tools/polly/compiler/emscripten.cmake
 create mode 100644 tools/polly/compiler/emscripten/glew/glewConfig.cmake
 create mode 100644 tools/polly/compiler/emscripten/glfw3/glfw3Config.cmake
 create mode 100644 tools/polly/compiler/gcc-5.cmake
 create mode 100644 tools/polly/compiler/gcc-6.cmake
 create mode 100644 tools/polly/compiler/gcc-7.cmake
 create mode 100644 tools/polly/compiler/gcc-8.cmake
 create mode 100644 tools/polly/compiler/gcc-cross-compile-raspberry-pi.cmake
 create mode 100644 tools/polly/compiler/gcc-cross-compile-simple-layout.cmake
 create mode 100644 tools/polly/compiler/gcc-cross-compile.cmake
 create mode 100644 tools/polly/compiler/gcc.cmake
 create mode 100644 tools/polly/compiler/gcc48.cmake
 create mode 100644 tools/polly/compiler/xcode.cmake
 create mode 100644 tools/polly/custom-libcxx.cmake
 create mode 100644 tools/polly/cxx11.cmake
 create mode 100644 tools/polly/cxx17.cmake
 create mode 100644 tools/polly/cygwin.cmake
 create mode 100644 tools/polly/default.cmake
 create mode 100644 tools/polly/docs/Makefile
 create mode 100644 tools/polly/docs/_fixme/Building-libcxx.md
 create mode 100644 tools/polly/docs/_fixme/Jenkins-(build-bot,-PR).md
 create mode 100644 tools/polly/docs/_fixme/Jenkins-(build-bot,-integration).md
 create mode 100644 tools/polly/docs/_fixme/Jenkins-(creating-user-on-linux-ubuntu).md
 create mode 100644 tools/polly/docs/_fixme/Jenkins-(creating-user-on-mac).md
 create mode 100644 tools/polly/docs/_fixme/Jenkins.md
 create mode 100644 tools/polly/docs/_fixme/Toolchain-list.md
 create mode 100644 tools/polly/docs/_fixme/Travis-CI-AppVeyor-support-table.md
 create mode 100644 tools/polly/docs/_fixme/Used-variables.md
 create mode 100644 tools/polly/docs/conf.py
 create mode 100644 tools/polly/docs/index.rst
 create mode 100755 tools/polly/docs/jenkins.sh
 create mode 100755 tools/polly/docs/make.sh
 create mode 100644 tools/polly/docs/requirements.txt
 create mode 100644 tools/polly/docs/screens/ios-team-id.png
 create mode 100644 tools/polly/docs/spelling.txt
 create mode 100644 tools/polly/docs/toolchains.rst
 create mode 100644 tools/polly/docs/toolchains/android.rst
 create mode 100644 tools/polly/docs/toolchains/android/developer-notes.rst
 create mode 100644 tools/polly/docs/toolchains/android/old.rst
 create mode 100644 tools/polly/docs/toolchains/clang-omp.rst
 create mode 100644 tools/polly/docs/toolchains/gcc-musl.rst
 create mode 100644 tools/polly/docs/toolchains/ios.rst
 create mode 100644 tools/polly/docs/toolchains/ios/bundle-id.rst
 create mode 100644 tools/polly/docs/toolchains/ios/errors/polly_ios_bundle_identifier.rst
 create mode 100644 tools/polly/docs/toolchains/ios/errors/polly_ios_development_team.rst
 create mode 100644 tools/polly/docs/toolchains/ios/errors/signing-request-development-team.rst
 create mode 100644 tools/polly/docs/toolchains/ios/screens/01_new_project.png
 create mode 100644 tools/polly/docs/toolchains/ios/screens/02_single_view_app.png
 create mode 100644 tools/polly/docs/toolchains/ios/screens/03_project_options.png
 create mode 100644 tools/polly/docs/toolchains/ios/screens/04_bundle_identifier.png
 create mode 100644 tools/polly/docs/toolchains/ios/screens/05_run_app.png
 create mode 100644 tools/polly/docs/toolchains/ios/screens/bad_bundle_id.png
 create mode 100644 tools/polly/docs/toolchains/linux-mingw-w64.rst
 create mode 100644 tools/polly/docs/toolchains/raspberry-pi.rst
 create mode 100644 tools/polly/emscripten-cxx11.cmake
 create mode 100644 tools/polly/emscripten-cxx14.cmake
 create mode 100644 tools/polly/emscripten-cxx17.cmake
 create mode 100644 tools/polly/examples/01-executable/CMakeLists.txt
 create mode 100644 tools/polly/examples/01-executable/main.cpp
 create mode 100644 tools/polly/examples/02-library/CMakeLists.txt
 create mode 100644 tools/polly/examples/02-library/foo.cpp
 create mode 100755 tools/polly/examples/02-library/main.cpp
 create mode 100644 tools/polly/examples/03-shared-link/CMakeLists.txt
 create mode 100644 tools/polly/examples/03-shared-link/boo.cpp
 create mode 100644 tools/polly/examples/03-shared-link/foo.cpp
 create mode 100755 tools/polly/examples/03-shared-link/main.cpp
 create mode 100644 tools/polly/examples/04-framework/CMakeLists.txt
 create mode 100644 tools/polly/examples/04-framework/foo.cpp
 create mode 100644 tools/polly/find/FindLibcxx.cmake
 create mode 100644 tools/polly/flags/32bit.cmake
 create mode 100644 tools/polly/flags/arm-linux-gnueabihf.cmake
 create mode 100644 tools/polly/flags/bitcode.cmake
 create mode 100644 tools/polly/flags/c11.cmake
 create mode 100644 tools/polly/flags/clang-tidy.cmake
 create mode 100644 tools/polly/flags/cxx11.cmake
 create mode 100644 tools/polly/flags/cxx14.cmake
 create mode 100644 tools/polly/flags/cxx17-gnu.cmake
 create mode 100644 tools/polly/flags/cxx17.cmake
 create mode 100644 tools/polly/flags/cxx20.cmake
 create mode 100644 tools/polly/flags/cxx98.cmake
 create mode 100644 tools/polly/flags/data-sections.cmake
 create mode 100644 tools/polly/flags/fconcepts.cmake
 create mode 100644 tools/polly/flags/fpic.cmake
 create mode 100644 tools/polly/flags/function-sections.cmake
 create mode 100644 tools/polly/flags/gnuxx11.cmake
 create mode 100644 tools/polly/flags/gold.cmake
 create mode 100644 tools/polly/flags/hardfloat.cmake
 create mode 100644 tools/polly/flags/hidden.cmake
 create mode 100644 tools/polly/flags/ios_nocodesign.cmake
 create mode 100644 tools/polly/flags/lld.cmake
 create mode 100644 tools/polly/flags/lto.cmake
 create mode 100644 tools/polly/flags/mtune_cortex-a15.cmake
 create mode 100644 tools/polly/flags/neon-vfpv4.cmake
 create mode 100644 tools/polly/flags/neon.cmake
 create mode 100644 tools/polly/flags/openwrt.cmake
 create mode 100644 tools/polly/flags/sanitize_address.cmake
 create mode 100644 tools/polly/flags/sanitize_leak.cmake
 create mode 100644 tools/polly/flags/sanitize_memory.cmake
 create mode 100644 tools/polly/flags/sanitize_thread.cmake
 create mode 100644 tools/polly/flags/static-std.cmake
 create mode 100644 tools/polly/flags/static.cmake
 create mode 100644 tools/polly/flags/vs-cxx14.cmake
 create mode 100644 tools/polly/flags/vs-cxx17.cmake
 create mode 100644 tools/polly/flags/vs-mt.cmake
 create mode 100644 tools/polly/flags/vs-nonpermissive.cmake
 create mode 100644 tools/polly/flags/vs-platform-win32.cmake
 create mode 100644 tools/polly/flags/vs-platform-x64.cmake
 create mode 100644 tools/polly/flags/vs-version-14-11.cmake
 create mode 100644 tools/polly/flags/vs-z7.cmake
 create mode 100644 tools/polly/flags/vs-zw.cmake
 create mode 100644 tools/polly/gcc-32bit-pic.cmake
 create mode 100644 tools/polly/gcc-32bit.cmake
 create mode 100644 tools/polly/gcc-4-8-c11.cmake
 create mode 100644 tools/polly/gcc-4-8-pic-hid-sections-cxx11-c11.cmake
 create mode 100644 tools/polly/gcc-4-8-pic-hid-sections.cmake
 create mode 100644 tools/polly/gcc-4-8-pic.cmake
 create mode 100644 tools/polly/gcc-4-8.cmake
 create mode 100644 tools/polly/gcc-5-cxx14-c11.cmake
 create mode 100644 tools/polly/gcc-5-pic-hid-sections-lto.cmake
 create mode 100644 tools/polly/gcc-5-pic-hid-sections.cmake
 create mode 100644 tools/polly/gcc-5.cmake
 create mode 100644 tools/polly/gcc-6-32bit-cxx14.cmake
 create mode 100644 tools/polly/gcc-7-cxx11-pic.cmake
 create mode 100644 tools/polly/gcc-7-cxx14-pic.cmake
 create mode 100644 tools/polly/gcc-7-cxx14.cmake
 create mode 100644 tools/polly/gcc-7-cxx17-concepts.cmake
 create mode 100644 tools/polly/gcc-7-cxx17-gnu.cmake
 create mode 100644 tools/polly/gcc-7-cxx17-pic.cmake
 create mode 100644 tools/polly/gcc-7-cxx17.cmake
 create mode 100644 tools/polly/gcc-7-pic-hid-sections-lto.cmake
 create mode 100644 tools/polly/gcc-7.cmake
 create mode 100644 tools/polly/gcc-8-cxx14-fpic.cmake
 create mode 100644 tools/polly/gcc-8-cxx14.cmake
 create mode 100644 tools/polly/gcc-8-cxx17-concepts.cmake
 create mode 100644 tools/polly/gcc-8-cxx17-fpic.cmake
 create mode 100644 tools/polly/gcc-8-cxx17.cmake
 create mode 100644 tools/polly/gcc-c11.cmake
 create mode 100644 tools/polly/gcc-cxx14-c11.cmake
 create mode 100644 tools/polly/gcc-cxx17-c11.cmake
 create mode 100644 tools/polly/gcc-cxx98.cmake
 create mode 100644 tools/polly/gcc-gold.cmake
 create mode 100644 tools/polly/gcc-hid-fpic.cmake
 create mode 100644 tools/polly/gcc-hid.cmake
 create mode 100644 tools/polly/gcc-lto.cmake
 create mode 100644 tools/polly/gcc-musl.cmake
 create mode 100644 tools/polly/gcc-ninja.cmake
 create mode 100644 tools/polly/gcc-pic-hid-sections-lto.cmake
 create mode 100644 tools/polly/gcc-pic-hid-sections.cmake
 create mode 100644 tools/polly/gcc-pic.cmake
 create mode 100644 tools/polly/gcc-static-std.cmake
 create mode 100644 tools/polly/gcc-static.cmake
 create mode 100644 tools/polly/gcc.cmake
 create mode 100644 tools/polly/ios-10-0-arm64-dep-8-0-hid-sections.cmake
 create mode 100644 tools/polly/ios-10-0-arm64.cmake
 create mode 100644 tools/polly/ios-10-0-armv7.cmake
 create mode 100644 tools/polly/ios-10-0-dep-8-0-hid-sections.cmake
 create mode 100644 tools/polly/ios-10-0-wo-armv7s.cmake
 create mode 100644 tools/polly/ios-10-0.cmake
 create mode 100644 tools/polly/ios-10-1-arm64-dep-8-0-hid-sections.cmake
 create mode 100644 tools/polly/ios-10-1-arm64.cmake
 create mode 100644 tools/polly/ios-10-1-armv7.cmake
 create mode 100644 tools/polly/ios-10-1-dep-8-0-hid-sections.cmake
 create mode 100644 tools/polly/ios-10-1-dep-8-0-libcxx-hid-sections-lto.cmake
 create mode 100644 tools/polly/ios-10-1-dep-8-0-libcxx-hid-sections.cmake
 create mode 100644 tools/polly/ios-10-1-wo-armv7s.cmake
 create mode 100644 tools/polly/ios-10-1.cmake
 create mode 100644 tools/polly/ios-10-2-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-10-2-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-10-2.cmake
 create mode 100644 tools/polly/ios-10-3-arm64.cmake
 create mode 100644 tools/polly/ios-10-3-armv7.cmake
 create mode 100644 tools/polly/ios-10-3-dep-8-0-bitcode.cmake
 create mode 100644 tools/polly/ios-10-3-dep-9-0-bitcode.cmake
 create mode 100644 tools/polly/ios-10-3-dep-9-3-i386-armv7.cmake
 create mode 100644 tools/polly/ios-10-3-dep-9-3-x86-64-arm64.cmake
 create mode 100644 tools/polly/ios-10-3-lto.cmake
 create mode 100644 tools/polly/ios-10-3.cmake
 create mode 100644 tools/polly/ios-11-0-dep-9-0-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-11-0-dep-9-0-device-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-11-0-dep-9-0-x86-64-arm64-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-11-0.cmake
 create mode 100644 tools/polly/ios-11-1-dep-9-0-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-11-1-dep-9-0-device-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-11-2-dep-9-0-device-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-11-2-dep-9-0-device-bitcode-nocxx.cmake
 create mode 100644 tools/polly/ios-11-2-dep-9-3-arm64-armv7.cmake
 create mode 100644 tools/polly/ios-11-3-dep-9-0-arm64.cmake
 create mode 100644 tools/polly/ios-11-3-dep-9-0-device-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-11-3-dep-9-0-device-bitcode-cxx17.cmake
 create mode 100644 tools/polly/ios-11-3-dep-9-0-device-bitcode-nocxx.cmake
 create mode 100644 tools/polly/ios-11-3-dep-9-0-device-bitcode.cmake
 create mode 100644 tools/polly/ios-11-3-dep-9-3-arm64-armv7.cmake
 create mode 100644 tools/polly/ios-11-4-dep-8-0-arm64-armv7-hid-sections-lto-cxx11.cmake
 create mode 100644 tools/polly/ios-11-4-dep-8-0-arm64-hid-sections-lto-cxx11.cmake
 create mode 100644 tools/polly/ios-11-4-dep-9-0-device-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-11-4-dep-9-0-device-bitcode-nocxx.cmake
 create mode 100644 tools/polly/ios-11-4-dep-9-3-arm64-armv7.cmake
 create mode 100644 tools/polly/ios-11-4-dep-9-3-arm64-hid-sections-lto-cxx11.cmake
 create mode 100644 tools/polly/ios-11-4-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-11-4-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-11-4-dep-9-3.cmake
 create mode 100644 tools/polly/ios-11-4-dep-9-4-arm64.cmake
 create mode 100644 tools/polly/ios-12-0-dep-11-0-arm64.cmake
 create mode 100644 tools/polly/ios-12-0-dep-9-0-device-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-12-1-dep-11-0-arm64.cmake
 create mode 100644 tools/polly/ios-12-1-dep-12-0-arm64-cxx17.cmake
 create mode 100644 tools/polly/ios-12-1-dep-9-0-device-bitcode-cxx14.cmake
 create mode 100644 tools/polly/ios-12-1-dep-9-0-device-bitcode-cxx17.cmake
 create mode 100644 tools/polly/ios-12-1-dep-9-3-arm64-bitcode.cmake
 create mode 100644 tools/polly/ios-12-1-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-12-1-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-12-1-dep-9-3-x86-64-arm64.cmake
 create mode 100644 tools/polly/ios-12-1-dep-9-3.cmake
 create mode 100644 tools/polly/ios-12-2-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-12-3-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-13-0-dep-10-0-arm64-bitcode-cxx17.cmake
 create mode 100644 tools/polly/ios-13-0-dep-11-0-arm64-bitcode-cxx17.cmake
 create mode 100644 tools/polly/ios-13-0-dep-9-3-arm64-bitcode.cmake
 create mode 100644 tools/polly/ios-13-0-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-13-2-dep-10-0-arm64-bitcode-cxx17.cmake
 create mode 100644 tools/polly/ios-13-2-dep-9-3-arm64-bitcode.cmake
 create mode 100644 tools/polly/ios-13-2-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-13-2-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-13-2-dep-9-3-armv7s.cmake
 create mode 100644 tools/polly/ios-13-2-dep-9-3-device-bitcode-cxx14.cmake
 create mode 100644 tools/polly/ios-13-2-dep-9-3-device-cxx14.cmake
 create mode 100644 tools/polly/ios-13-3-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-13-3-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-13-3-dep-9-3-armv7s.cmake
 create mode 100644 tools/polly/ios-13-3-dep-9-3-device-bitcode-cxx14.cmake
 create mode 100644 tools/polly/ios-13-3-dep-9-3-device-cxx14.cmake
 create mode 100644 tools/polly/ios-13-4-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-13-4-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-13-4-dep-9-3-armv7s.cmake
 create mode 100644 tools/polly/ios-13-4-dep-9-3-device-bitcode-cxx14.cmake
 create mode 100644 tools/polly/ios-13-4-dep-9-3-device-cxx14.cmake
 create mode 100644 tools/polly/ios-13-5-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-13-5-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-13-5-dep-9-3-armv7s.cmake
 create mode 100644 tools/polly/ios-13-5-dep-9-3-device-bitcode-cxx14.cmake
 create mode 100644 tools/polly/ios-13-5-dep-9-3-device-cxx14.cmake
 create mode 100644 tools/polly/ios-13-6-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-13-6-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-13-6-dep-9-3-armv7s.cmake
 create mode 100644 tools/polly/ios-13-6-dep-9-3-device-bitcode-cxx14.cmake
 create mode 100644 tools/polly/ios-13-6-dep-9-3-device-cxx14.cmake
 create mode 100644 tools/polly/ios-14-0-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-14-0-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-14-0-dep-9-3-armv7s.cmake
 create mode 100644 tools/polly/ios-14-0-dep-9-3-device-bitcode-cxx14.cmake
 create mode 100644 tools/polly/ios-14-0-dep-9-3-device-cxx14.cmake
 create mode 100644 tools/polly/ios-14-2-dep-10-0-arm64.cmake
 create mode 100644 tools/polly/ios-14-2-dep-10-0-armv7.cmake
 create mode 100644 tools/polly/ios-14-2-dep-10-0-armv7s.cmake
 create mode 100644 tools/polly/ios-14-2-dep-10-0-device-bitcode-cxx14.cmake
 create mode 100644 tools/polly/ios-14-2-dep-10-0-device-cxx14.cmake
 create mode 100644 tools/polly/ios-14-3-dep-10-0-arm64.cmake
 create mode 100644 tools/polly/ios-14-3-dep-10-0-armv7.cmake
 create mode 100644 tools/polly/ios-14-3-dep-10-0-armv7s.cmake
 create mode 100644 tools/polly/ios-14-3-dep-10-0-device-bitcode-cxx14.cmake
 create mode 100644 tools/polly/ios-14-3-dep-10-0-device-cxx14.cmake
 create mode 100644 tools/polly/ios-14-4-dep-10-0-arm64.cmake
 create mode 100644 tools/polly/ios-14-4-dep-10-0-armv7.cmake
 create mode 100644 tools/polly/ios-14-4-dep-10-0-armv7s.cmake
 create mode 100644 tools/polly/ios-14-4-dep-10-0-device-bitcode-cxx14.cmake
 create mode 100644 tools/polly/ios-14-4-dep-10-0-device-cxx14.cmake
 create mode 100644 tools/polly/ios-7-0.cmake
 create mode 100644 tools/polly/ios-7-1.cmake
 create mode 100644 tools/polly/ios-8-0.cmake
 create mode 100644 tools/polly/ios-8-1.cmake
 create mode 100644 tools/polly/ios-8-2-arm64-hid.cmake
 create mode 100644 tools/polly/ios-8-2-arm64.cmake
 create mode 100644 tools/polly/ios-8-2-cxx98.cmake
 create mode 100644 tools/polly/ios-8-2-i386-arm64.cmake
 create mode 100644 tools/polly/ios-8-2.cmake
 create mode 100644 tools/polly/ios-8-4-arm64.cmake
 create mode 100644 tools/polly/ios-8-4-armv7.cmake
 create mode 100644 tools/polly/ios-8-4-armv7s.cmake
 create mode 100644 tools/polly/ios-8-4-hid.cmake
 create mode 100644 tools/polly/ios-8-4.cmake
 create mode 100644 tools/polly/ios-9-0-armv7.cmake
 create mode 100644 tools/polly/ios-9-0-dep-7-0-armv7.cmake
 create mode 100644 tools/polly/ios-9-0-i386-armv7.cmake
 create mode 100644 tools/polly/ios-9-0-wo-armv7s.cmake
 create mode 100644 tools/polly/ios-9-0.cmake
 create mode 100644 tools/polly/ios-9-1-arm64.cmake
 create mode 100644 tools/polly/ios-9-1-armv7.cmake
 create mode 100644 tools/polly/ios-9-1-dep-7-0-armv7.cmake
 create mode 100644 tools/polly/ios-9-1-dep-8-0-hid.cmake
 create mode 100644 tools/polly/ios-9-1-hid.cmake
 create mode 100644 tools/polly/ios-9-1.cmake
 create mode 100644 tools/polly/ios-9-2-arm64.cmake
 create mode 100644 tools/polly/ios-9-2-armv7.cmake
 create mode 100644 tools/polly/ios-9-2-hid-sections.cmake
 create mode 100644 tools/polly/ios-9-2-hid.cmake
 create mode 100644 tools/polly/ios-9-2.cmake
 create mode 100644 tools/polly/ios-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-9-3-wo-armv7s.cmake
 create mode 100644 tools/polly/ios-9-3.cmake
 create mode 100755 tools/polly/ios-bitcode.cmake
 create mode 100644 tools/polly/ios-cxx17.cmake
 create mode 100644 tools/polly/ios-dep-10-0-bitcode-cxx17.cmake
 create mode 100644 tools/polly/ios-dep-11-0-bitcode-cxx17.cmake
 create mode 100644 tools/polly/ios-dep-12-0-bitcode-cxx17.cmake
 create mode 100644 tools/polly/ios-dep-8-0-arm64-armv7-hid-sections-cxx11.cmake
 create mode 100644 tools/polly/ios-dep-8-0-arm64-armv7-hid-sections-lto-cxx11.cmake
 create mode 100644 tools/polly/ios-dep-8-0-arm64-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-0-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-0-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-0-wo-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-0.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-1-arm64-dep-9-0-device-libcxx-hid-sections-lto.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-1-arm64-dep-9-0-device-libcxx-hid-sections.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-1-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-1-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-1-dep-8-0-device-libcxx-hid-sections-lto.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-1-dep-8-0-libcxx-hid-sections-lto.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-1-dep-9-0-device-libcxx-hid-sections-lto.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-1-wo-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-1.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-2.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-3-arm64-dep-9-0-device-libcxx-hid-sections.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-3-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-3-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-3-cxx14.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-3-dep-9-0-bitcode.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-3-wo-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-3.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-0-arm64-dep-9-0-device-libcxx-hid-sections.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-0-dep-9-0-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-0.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-1-dep-9-0-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-1-dep-9-0-wo-armv7s-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-1.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-2-dep-8-0-wo-armv7s-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-2-dep-9-0-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-2-dep-9-3-arm64-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-2-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-2-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-2-dep-9-3-i386-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-2-dep-9-3.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-2.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-3-dep-9-0-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-3-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-3-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-3-dep-9-3.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-4-dep-9-0-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-4-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-4-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-4-dep-9-3.cmake
 create mode 100644 tools/polly/ios-nocodesign-12-0-dep-10-0-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-12-0-dep-9-0-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-12-1-dep-9-0-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-12-1-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-12-1.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-0-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-2-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-2-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-2-dep-9-3-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-2-dep-9-3-device-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-2-dep-9-3-device.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-2-dep-9-3.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-5-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-5-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-5-dep-9-3-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-5-dep-9-3-device-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-5-dep-9-3-device.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-5-dep-9-3.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-6-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-6-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-6-dep-9-3-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-6-dep-9-3-device-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-6-dep-9-3-device.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-6-dep-9-3.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-0-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-0-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-0-dep-9-3-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-0-dep-9-3-device-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-0-dep-9-3-device.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-0-dep-9-3.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-2-dep-10-0-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-2-dep-10-0-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-2-dep-10-0-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-2-dep-10-0-device-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-2-dep-10-0-device.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-2-dep-10-0.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-3-dep-10-0-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-3-dep-10-0-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-3-dep-10-0-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-3-dep-10-0-device-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-3-dep-10-0-device.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-3-dep-10-0.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-4-dep-10-0-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-4-dep-10-0-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-4-dep-10-0-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-4-dep-10-0-device-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-4-dep-10-0-device.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-4-dep-10-0.cmake
 create mode 100644 tools/polly/ios-nocodesign-8-1.cmake
 create mode 100644 tools/polly/ios-nocodesign-8-4.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-1-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-1-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-1.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-2-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-2-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-2.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-3-device-hid-sections.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-3-device.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-3-wo-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-3.cmake
 create mode 100644 tools/polly/ios-nocodesign-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-dep-9-0-cxx14.cmake
 create mode 100644 tools/polly/ios-nocodesign-hid-sections.cmake
 create mode 100644 tools/polly/ios-nocodesign-wo-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign.cmake
 create mode 100644 tools/polly/ios.cmake
 create mode 100644 tools/polly/libcxx-fpic-hid-sections.cmake
 create mode 100644 tools/polly/libcxx-hid-fpic.cmake
 create mode 100644 tools/polly/libcxx-hid-sections.cmake
 create mode 100644 tools/polly/libcxx-hid.cmake
 create mode 100644 tools/polly/libcxx-no-sdk.cmake
 create mode 100644 tools/polly/libcxx.cmake
 create mode 100644 tools/polly/libcxx14.cmake
 create mode 100644 tools/polly/library/std/libcxx.cmake
 create mode 100644 tools/polly/library/std/libstdcxx.cmake
 create mode 100644 tools/polly/library/std/nolibs.cmake
 create mode 100644 tools/polly/linux-gcc-armhf-neon-vfpv4.cmake
 create mode 100644 tools/polly/linux-gcc-armhf-neon.cmake
 create mode 100644 tools/polly/linux-gcc-armhf.cmake
 create mode 100644 tools/polly/linux-gcc-jetson-tk1.cmake
 create mode 100644 tools/polly/linux-gcc-x64.cmake
 create mode 100644 tools/polly/linux-mingw-w32-cxx14.cmake
 create mode 100644 tools/polly/linux-mingw-w32.cmake
 create mode 100644 tools/polly/linux-mingw-w64-cxx14.cmake
 create mode 100644 tools/polly/linux-mingw-w64-cxx98.cmake
 create mode 100644 tools/polly/linux-mingw-w64-gnuxx11.cmake
 create mode 100644 tools/polly/linux-mingw-w64.cmake
 create mode 100644 tools/polly/mingw-c11.cmake
 create mode 100644 tools/polly/mingw-cxx14.cmake
 create mode 100644 tools/polly/mingw-cxx17.cmake
 create mode 100644 tools/polly/mingw.cmake
 create mode 100644 tools/polly/msys-cxx14.cmake
 create mode 100644 tools/polly/msys-cxx17.cmake
 create mode 100644 tools/polly/msys.cmake
 create mode 100644 tools/polly/ninja-gcc-7-cxx17-concepts.cmake
 create mode 100644 tools/polly/ninja-gcc-8-cxx17-concepts.cmake
 create mode 100644 tools/polly/ninja-vs-12-2013-win64.cmake
 create mode 100644 tools/polly/ninja-vs-14-2015-win64.cmake
 create mode 100644 tools/polly/ninja-vs-15-2017-win64-cxx17-nonpermissive.cmake
 create mode 100644 tools/polly/ninja-vs-15-2017-win64-cxx17.cmake
 create mode 100644 tools/polly/ninja-vs-15-2017-win64.cmake
 create mode 100644 tools/polly/nmake-vs-12-2013-win64.cmake
 create mode 100644 tools/polly/nmake-vs-12-2013.cmake
 create mode 100644 tools/polly/nmake-vs-15-2017-win64-cxx17-nonpermissive.cmake
 create mode 100644 tools/polly/nmake-vs-15-2017-win64-cxx17.cmake
 create mode 100644 tools/polly/nmake-vs-15-2017-win64.cmake
 create mode 100644 tools/polly/openbsd-egcc-cxx11-static-std.cmake
 create mode 100644 tools/polly/os/android.cmake
 create mode 100644 tools/polly/os/cygwin.cmake
 create mode 100644 tools/polly/os/iphone-default-sdk.cmake
 create mode 100644 tools/polly/os/iphone.cmake
 create mode 100644 tools/polly/os/osx.cmake
 create mode 100644 tools/polly/os/raspberry-pi-hardfloat.cmake
 create mode 100644 tools/polly/os/raspberry-pi1.cmake
 create mode 100644 tools/polly/os/raspberry-pi2.cmake
 create mode 100644 tools/polly/os/raspberry-pi3.cmake
 create mode 100644 tools/polly/os/rpi-sysroot.cmake
 create mode 100644 tools/polly/os/vc-mdd-android.cmake
 create mode 100644 tools/polly/osx-10-10-dep-10-7.cmake
 create mode 100644 tools/polly/osx-10-10-dep-10-9-make.cmake
 create mode 100644 tools/polly/osx-10-10.cmake
 create mode 100644 tools/polly/osx-10-11-hid-sections-lto.cmake
 create mode 100644 tools/polly/osx-10-11-hid-sections.cmake
 create mode 100644 tools/polly/osx-10-11-lto.cmake
 create mode 100644 tools/polly/osx-10-11-make.cmake
 create mode 100644 tools/polly/osx-10-11-sanitize-address.cmake
 create mode 100644 tools/polly/osx-10-11.cmake
 create mode 100644 tools/polly/osx-10-12-cxx14.cmake
 create mode 100644 tools/polly/osx-10-12-cxx17.cmake
 create mode 100644 tools/polly/osx-10-12-cxx98.cmake
 create mode 100644 tools/polly/osx-10-12-dep-10-10-lto.cmake
 create mode 100644 tools/polly/osx-10-12-dep-10-10.cmake
 create mode 100644 tools/polly/osx-10-12-hid-sections.cmake
 create mode 100644 tools/polly/osx-10-12-lto.cmake
 create mode 100644 tools/polly/osx-10-12-make.cmake
 create mode 100644 tools/polly/osx-10-12-ninja.cmake
 create mode 100644 tools/polly/osx-10-12-sanitize-address-hid-sections.cmake
 create mode 100644 tools/polly/osx-10-12-sanitize-address.cmake
 create mode 100644 tools/polly/osx-10-12.cmake
 create mode 100644 tools/polly/osx-10-13-cxx14.cmake
 create mode 100644 tools/polly/osx-10-13-cxx17.cmake
 create mode 100644 tools/polly/osx-10-13-dep-10-10-cxx14.cmake
 create mode 100644 tools/polly/osx-10-13-dep-10-10-cxx17.cmake
 create mode 100644 tools/polly/osx-10-13-dep-10-10.cmake
 create mode 100644 tools/polly/osx-10-13-dep-10-12-cxx14.cmake
 create mode 100644 tools/polly/osx-10-13-dep-10-12-cxx17.cmake
 create mode 100644 tools/polly/osx-10-13-dep-10-12.cmake
 create mode 100644 tools/polly/osx-10-13-i386-cxx14.cmake
 create mode 100644 tools/polly/osx-10-13-make-cxx14.cmake
 create mode 100644 tools/polly/osx-10-13.cmake
 create mode 100644 tools/polly/osx-10-14-cxx14.cmake
 create mode 100644 tools/polly/osx-10-14-cxx17.cmake
 create mode 100644 tools/polly/osx-10-14-dep-10-10-cxx14.cmake
 create mode 100644 tools/polly/osx-10-14-dep-10-10-cxx17.cmake
 create mode 100644 tools/polly/osx-10-14-dep-10-10.cmake
 create mode 100644 tools/polly/osx-10-14-dep-10-12-cxx14.cmake
 create mode 100644 tools/polly/osx-10-14-dep-10-12-cxx17.cmake
 create mode 100644 tools/polly/osx-10-14-dep-10-12.cmake
 create mode 100644 tools/polly/osx-10-14.cmake
 create mode 100644 tools/polly/osx-10-15-cxx17.cmake
 create mode 100644 tools/polly/osx-10-15-dep-10-10-cxx14.cmake
 create mode 100644 tools/polly/osx-10-15-dep-10-10-cxx17.cmake
 create mode 100644 tools/polly/osx-10-15-dep-10-10.cmake
 create mode 100644 tools/polly/osx-10-15-dep-10-12-cxx17.cmake
 create mode 100644 tools/polly/osx-10-15.cmake
 create mode 100644 tools/polly/osx-10-7.cmake
 create mode 100644 tools/polly/osx-10-8.cmake
 create mode 100644 tools/polly/osx-10-9.cmake
 create mode 100644 tools/polly/osx-11-0-cxx17.cmake
 create mode 100644 tools/polly/osx-11-0-dep-10-10-cxx17.cmake
 create mode 100644 tools/polly/osx-11-0.cmake
 create mode 100644 tools/polly/osx-11-1-cxx17.cmake
 create mode 100644 tools/polly/osx-11-1-dep-10-10-cxx17.cmake
 create mode 100644 tools/polly/osx-11-1.cmake
 create mode 100644 tools/polly/raspberrypi1-cxx11-pic-static-std.cmake
 create mode 100644 tools/polly/raspberrypi1-cxx11-pic.cmake
 create mode 100644 tools/polly/raspberrypi1-cxx14-pic-static-std.cmake
 create mode 100644 tools/polly/raspberrypi2-cxx11-pic.cmake
 create mode 100644 tools/polly/raspberrypi2-cxx11.cmake
 create mode 100644 tools/polly/raspberrypi3-clang-cxx11.cmake
 create mode 100644 tools/polly/raspberrypi3-clang-cxx14-pic.cmake
 create mode 100644 tools/polly/raspberrypi3-clang-cxx14.cmake
 create mode 100644 tools/polly/raspberrypi3-cxx11.cmake
 create mode 100644 tools/polly/raspberrypi3-cxx14.cmake
 create mode 100644 tools/polly/raspberrypi3-gcc-pic-hid-sections.cmake
 create mode 100644 tools/polly/sanitize-address-cxx17-pic.cmake
 create mode 100644 tools/polly/sanitize-address-cxx17.cmake
 create mode 100644 tools/polly/sanitize-address.cmake
 create mode 100644 tools/polly/sanitize-leak-cxx17-pic.cmake
 create mode 100644 tools/polly/sanitize-leak-cxx17.cmake
 create mode 100644 tools/polly/sanitize-leak.cmake
 create mode 100644 tools/polly/sanitize-memory.cmake
 create mode 100644 tools/polly/sanitize-thread-cxx17-pic.cmake
 create mode 100644 tools/polly/sanitize-thread-cxx17.cmake
 create mode 100644 tools/polly/sanitize-thread.cmake
 create mode 100644 tools/polly/scripts/Info.plist
 create mode 100644 tools/polly/scripts/NoCodeSign.xcconfig
 create mode 100755 tools/polly/scripts/clang-analyze.sh
 create mode 100755 tools/polly/scripts/clangxx-analyze.sh
 create mode 100644 tools/polly/utilities/polly_add_cache_flag.cmake
 create mode 100644 tools/polly/utilities/polly_clear_environment_variables.cmake
 create mode 100644 tools/polly/utilities/polly_common.cmake
 create mode 100644 tools/polly/utilities/polly_fatal_error.cmake
 create mode 100644 tools/polly/utilities/polly_init.cmake
 create mode 100644 tools/polly/utilities/polly_ios_bundle_identifier.cmake
 create mode 100644 tools/polly/utilities/polly_ios_development_team.cmake
 create mode 100644 tools/polly/utilities/polly_module_path.cmake
 create mode 100644 tools/polly/utilities/polly_status_debug.cmake
 create mode 100644 tools/polly/utilities/polly_status_print.cmake
 create mode 100644 tools/polly/vs-10-2010.cmake
 create mode 100644 tools/polly/vs-11-2012-arm.cmake
 create mode 100644 tools/polly/vs-11-2012-win64.cmake
 create mode 100644 tools/polly/vs-11-2012.cmake
 create mode 100644 tools/polly/vs-12-2013-arm.cmake
 create mode 100644 tools/polly/vs-12-2013-mt.cmake
 create mode 100644 tools/polly/vs-12-2013-win64.cmake
 create mode 100644 tools/polly/vs-12-2013-xp.cmake
 create mode 100644 tools/polly/vs-12-2013.cmake
 create mode 100644 tools/polly/vs-14-2015-arm.cmake
 create mode 100644 tools/polly/vs-14-2015-sdk-8-1.cmake
 create mode 100644 tools/polly/vs-14-2015-win64-sdk-8-1.cmake
 create mode 100644 tools/polly/vs-14-2015-win64.cmake
 create mode 100644 tools/polly/vs-14-2015.cmake
 create mode 100644 tools/polly/vs-15-2017-cxx14-mt.cmake
 create mode 100644 tools/polly/vs-15-2017-cxx17.cmake
 create mode 100644 tools/polly/vs-15-2017-mt.cmake
 create mode 100644 tools/polly/vs-15-2017-store-10-zw.cmake
 create mode 100644 tools/polly/vs-15-2017-win64-cxx14-mt.cmake
 create mode 100644 tools/polly/vs-15-2017-win64-cxx14.cmake
 create mode 100644 tools/polly/vs-15-2017-win64-cxx17-nonpermissive.cmake
 create mode 100644 tools/polly/vs-15-2017-win64-cxx17.cmake
 create mode 100644 tools/polly/vs-15-2017-win64-llvm-vs2014.cmake
 create mode 100644 tools/polly/vs-15-2017-win64-llvm.cmake
 create mode 100644 tools/polly/vs-15-2017-win64-mt.cmake
 create mode 100644 tools/polly/vs-15-2017-win64-store-10-cxx17.cmake
 create mode 100644 tools/polly/vs-15-2017-win64-store-10-zw.cmake
 create mode 100644 tools/polly/vs-15-2017-win64-version-14-11.cmake
 create mode 100644 tools/polly/vs-15-2017-win64-z7.cmake
 create mode 100644 tools/polly/vs-15-2017-win64.cmake
 create mode 100644 tools/polly/vs-15-2017.cmake
 create mode 100644 tools/polly/vs-16-2019-cxx14.cmake
 create mode 100644 tools/polly/vs-16-2019-cxx17.cmake
 create mode 100644 tools/polly/vs-16-2019-llvm-cxx17.cmake
 create mode 100644 tools/polly/vs-16-2019-win64-cxx14.cmake
 create mode 100644 tools/polly/vs-16-2019-win64-cxx17.cmake
 create mode 100644 tools/polly/vs-16-2019-win64-llvm-cxx17.cmake
 create mode 100644 tools/polly/vs-16-2019-win64.cmake
 create mode 100644 tools/polly/vs-16-2019.cmake
 create mode 100644 tools/polly/vs-8-2005.cmake
 create mode 100644 tools/polly/vs-9-2008.cmake
 create mode 100644 tools/polly/xcode-cxx17.cmake
 create mode 100644 tools/polly/xcode-cxx98.cmake
 create mode 100644 tools/polly/xcode-gcc.cmake
 create mode 100644 tools/polly/xcode-hid-sections.cmake
 create mode 100644 tools/polly/xcode-nocxx.cmake
 create mode 100644 tools/polly/xcode-sections.cmake
 create mode 100644 tools/polly/xcode.cmake
 ~/ Kinpach/workspace/TimpLab08$ git pull --rebase origin master 
Из https://github.com/ Kinpach/TimpLab08
 * branch            master     -> FETCH_HEAD
Автослияние README.md
КОНФЛИКТ (содержимое): Конфликт слияния в README.md
error: не удалось применить коммит a88a687... adding Dockerfile
подсказка: Resolve all conflicts manually, mark them as resolved with
подсказка: "git add/rm <conflicted_files>", then run "git rebase --continue".
подсказка: You can instead skip this commit: run "git rebase --skip".
подсказка: To abort and get back to the state before "git rebase", run "git rebase --abort".
Не удалось применить коммит a88a687... adding Dockerfile
 ~/ Kinpach/workspace/TimpLab08$ git pull --rebase origin master
error: Невозможно выполнить получение, так как у вас имеются не слитые файлы.
подсказка: Исправьте их в рабочем каталоге, затем запустите «git add/rm <файл>»,
подсказка: чтобы пометить исправление и сделайте коммит.
fatal: Выход из-за неразрешенного конфликта.
 ~/ Kinpach/workspace/TimpLab08$ git add .
 ~/ Kinpach/workspace/TimpLab08$ git rebase --continue
[отделённый HEAD fc072b9] adding Dockerfile
 810 files changed, 31193 insertions(+), 310 deletions(-)
 create mode 100644 cmake/Hunter/config.cmake
 create mode 100644 cmake/HunterGate.cmake
 create mode 100644 demo/main.cpp
 create mode 100644 logs/log.txt
 create mode 100644 tools/polly/.gitignore
 create mode 100644 tools/polly/.gitmodules
 create mode 100644 tools/polly/.travis.yml
 create mode 100644 tools/polly/CONTRIBUTING.md
 create mode 100644 tools/polly/LICENSE
 create mode 100644 tools/polly/analyze-cxx17.cmake
 create mode 100644 tools/polly/analyze.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-16-armeabi-v7a-neon-clang-35-hid-sections.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-16-armeabi-v7a-neon-clang-35-hid.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-16-armeabi-v7a-neon-clang-35.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-16-armeabi-v7a-neon.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-16-x86-hid-sections.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-16-x86-hid.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-16-x86.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-19-armeabi-v7a-neon-c11.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-19-armeabi-v7a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-19-armeabi-v7a-neon-hid-sections-lto.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-19-armeabi-v7a-neon-hid-sections.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-19-armeabi-v7a-neon.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-arm64-v8a-clang-35.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-arm64-v8a-gcc-49-hid-sections.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-arm64-v8a-gcc-49-hid.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-arm64-v8a-gcc-49.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-arm64-v8a.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-armeabi-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-armeabi-v7a-neon-clang-35.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-armeabi-v7a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-armeabi-v7a-neon-hid-sections.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-armeabi-v7a-neon.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-armeabi-v7a.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-armeabi.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-mips-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-mips.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-mips64.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-x86-64-hid-sections.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-x86-64-hid.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-x86-64.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-x86-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-21-x86.cmake
 create mode 100644 tools/polly/android-ndk-r10e-api-8-armeabi-v7a.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-16-armeabi-cxx14.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-16-armeabi-v7a-cxx14.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-16-armeabi-v7a-neon-clang-35-hid.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-16-armeabi-v7a-neon-clang-35.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-16-armeabi-v7a-neon-cxx14.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-16-armeabi-v7a-neon.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-16-armeabi-v7a.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-16-armeabi.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-16-x86-hid.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-16-x86.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-19-armeabi-v7a-neon.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-arm64-v8a-clang-35.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-arm64-v8a-gcc-49-hid.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-arm64-v8a-gcc-49.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-arm64-v8a.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-armeabi-v7a-neon-clang-35.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-armeabi-v7a-neon.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-armeabi-v7a.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-armeabi.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-mips.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-mips64.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-x86-64-hid.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-x86-64.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-21-x86.cmake
 create mode 100644 tools/polly/android-ndk-r11c-api-8-armeabi-v7a.cmake
 create mode 100644 tools/polly/android-ndk-r12b-api-19-armeabi-v7a-neon.cmake
 create mode 100644 tools/polly/android-ndk-r13b-api-19-armeabi-v7a-neon.cmake
 create mode 100644 tools/polly/android-ndk-r14-api-16-armeabi-v7a-neon-clang-hid-sections-lto.cmake
 create mode 100644 tools/polly/android-ndk-r14-api-19-armeabi-v7a-neon-c11.cmake
 create mode 100644 tools/polly/android-ndk-r14-api-19-armeabi-v7a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r14-api-19-armeabi-v7a-neon-clang.cmake
 create mode 100644 tools/polly/android-ndk-r14-api-19-armeabi-v7a-neon-hid-sections-lto.cmake
 create mode 100644 tools/polly/android-ndk-r14-api-21-arm64-v8a-clang-hid-sections-lto.cmake
 create mode 100644 tools/polly/android-ndk-r14-api-21-arm64-v8a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r14-api-21-x86-64.cmake
 create mode 100644 tools/polly/android-ndk-r14b-api-21-armeabi-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r14b-api-21-armeabi-v7a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r14b-api-21-mips-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r14b-api-21-x86-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-16-armeabi-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-16-armeabi-v7a-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-16-armeabi-v7a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-16-mips-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-16-x86-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-21-arm64-v8a-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-21-arm64-v8a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-21-armeabi-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-21-armeabi-v7a-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-21-armeabi-v7a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-21-mips-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-21-x86-64-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-21-x86-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r15c-api-24-armeabi-v7a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-16-armeabi-v7a-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-16-armeabi-v7a-thumb-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-16-x86-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-19-gcc-49-armeabi-v7a-neon-libcxx-hid-sections-lto.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-21-arm64-v8a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-21-arm64-v8a-neon-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-21-armeabi-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-21-armeabi-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-21-armeabi-v7a-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-21-armeabi-v7a-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-21-armeabi-v7a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-21-armeabi-v7a-neon-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-21-x86-64-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-21-x86-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-24-arm64-v8a-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-24-armeabi-v7a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-24-armeabi-v7a-neon-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-24-x86-64-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r16b-api-24-x86-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r17-api-16-armeabi-v7a-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r17-api-16-x86-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r17-api-19-armeabi-v7a-neon-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r17-api-19-armeabi-v7a-neon-hid-sections.cmake
 create mode 100644 tools/polly/android-ndk-r17-api-21-arm64-v8a-neon-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r17-api-21-x86-64-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r17-api-24-arm64-v8a-clang-libcxx11.cmake
 create mode 100644 tools/polly/android-ndk-r17-api-24-arm64-v8a-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r18-api-24-arm64-v8a-clang-libcxx14.cmake
 create mode 100644 tools/polly/android-ndk-r18b-api-16-armeabi-v7a-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r18b-api-21-arm64-v8a-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r18b-api-21-armeabi-v7a-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r18b-api-21-x86-64-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r18b-api-21-x86-clang-libcxx.cmake
 create mode 100644 tools/polly/android-ndk-r18b-api-24-arm64-v8a-clang-libcxx11.cmake
 create mode 100644 tools/polly/android-ndk-r18b-api-28-arm64-v8a-clang-libcxx11.cmake
 create mode 100644 tools/polly/android-vc-ndk-r10e-api-19-arm-clang-3-6.cmake
 create mode 100644 tools/polly/android-vc-ndk-r10e-api-19-arm-gcc-4-9.cmake
 create mode 100644 tools/polly/android-vc-ndk-r10e-api-19-x86-clang-3-6.cmake
 create mode 100644 tools/polly/android-vc-ndk-r10e-api-21-arm-clang-3-6.cmake
 create mode 100644 tools/polly/appveyor.yml
 create mode 100644 tools/polly/arm-openwrt-linux-muslgnueabi-cxx14.cmake
 create mode 100644 tools/polly/arm-openwrt-linux-muslgnueabi.cmake
 create mode 100755 tools/polly/bin/build.py
 create mode 100644 tools/polly/bin/detail/__init__.py
 create mode 100644 tools/polly/bin/detail/call.py
 create mode 100644 tools/polly/bin/detail/cpack_generator.py
 create mode 100644 tools/polly/bin/detail/create_archive.py
 create mode 100644 tools/polly/bin/detail/create_framework.py
 create mode 100644 tools/polly/bin/detail/generate_command.py
 create mode 100755 tools/polly/bin/detail/get_nmake_environment.py
 create mode 100644 tools/polly/bin/detail/ios_dev_root.py
 create mode 100644 tools/polly/bin/detail/logging.py
 create mode 100644 tools/polly/bin/detail/open_project.py
 create mode 100644 tools/polly/bin/detail/osx_dev_root.py
 create mode 100644 tools/polly/bin/detail/pack_command.py
 create mode 100644 tools/polly/bin/detail/rmtree.py
 create mode 100644 tools/polly/bin/detail/target.py
 create mode 100644 tools/polly/bin/detail/test_command.py
 create mode 100644 tools/polly/bin/detail/timer.py
 create mode 100644 tools/polly/bin/detail/toolchain_name.py
 create mode 100644 tools/polly/bin/detail/toolchain_table.py
 create mode 100755 tools/polly/bin/detail/util.py
 create mode 100644 tools/polly/bin/detail/verify_mingw_path.py
 create mode 100644 tools/polly/bin/detail/verify_msys_path.py
 create mode 100755 tools/polly/bin/detail/win32.py
 create mode 100755 tools/polly/bin/install-ci-dependencies.py
 create mode 100755 tools/polly/bin/polly
 create mode 100644 tools/polly/bin/polly.bat
 create mode 100755 tools/polly/bin/polly.py
 create mode 100644 tools/polly/clang-5-cxx14.cmake
 create mode 100644 tools/polly/clang-5-cxx17.cmake
 create mode 100644 tools/polly/clang-5.cmake
 create mode 100644 tools/polly/clang-cxx11.cmake
 create mode 100644 tools/polly/clang-cxx14-pic.cmake
 create mode 100644 tools/polly/clang-cxx14.cmake
 create mode 100644 tools/polly/clang-cxx17-pic.cmake
 create mode 100644 tools/polly/clang-cxx17.cmake
 create mode 100644 tools/polly/clang-cxx20.cmake
 create mode 100644 tools/polly/clang-fpic-hid-sections.cmake
 create mode 100644 tools/polly/clang-fpic-static-std-cxx14.cmake
 create mode 100644 tools/polly/clang-fpic-static-std.cmake
 create mode 100644 tools/polly/clang-fpic.cmake
 create mode 100644 tools/polly/clang-libcxx-fpic.cmake
 create mode 100644 tools/polly/clang-libcxx.cmake
 create mode 100644 tools/polly/clang-libcxx14-fpic.cmake
 create mode 100644 tools/polly/clang-libcxx14.cmake
 create mode 100644 tools/polly/clang-libcxx17-fpic.cmake
 create mode 100644 tools/polly/clang-libcxx17-static.cmake
 create mode 100644 tools/polly/clang-libcxx17.cmake
 create mode 100644 tools/polly/clang-libcxx98.cmake
 create mode 100644 tools/polly/clang-libstdcxx.cmake
 create mode 100644 tools/polly/clang-lto.cmake
 create mode 100644 tools/polly/clang-omp.cmake
 create mode 100644 tools/polly/clang-tidy-libcxx.cmake
 create mode 100644 tools/polly/clang-tidy.cmake
 create mode 100644 tools/polly/compiler/cl.cmake
 create mode 100644 tools/polly/compiler/clang-5.cmake
 create mode 100644 tools/polly/compiler/clang-omp.cmake
 create mode 100644 tools/polly/compiler/clang-tools.cmake
 create mode 100644 tools/polly/compiler/clang.cmake
 create mode 100644 tools/polly/compiler/egcc.cmake
 create mode 100644 tools/polly/compiler/emscripten.cmake
 create mode 100644 tools/polly/compiler/emscripten/glew/glewConfig.cmake
 create mode 100644 tools/polly/compiler/emscripten/glfw3/glfw3Config.cmake
 create mode 100644 tools/polly/compiler/gcc-5.cmake
 create mode 100644 tools/polly/compiler/gcc-6.cmake
 create mode 100644 tools/polly/compiler/gcc-7.cmake
 create mode 100644 tools/polly/compiler/gcc-8.cmake
 create mode 100644 tools/polly/compiler/gcc-cross-compile-raspberry-pi.cmake
 create mode 100644 tools/polly/compiler/gcc-cross-compile-simple-layout.cmake
 create mode 100644 tools/polly/compiler/gcc-cross-compile.cmake
 create mode 100644 tools/polly/compiler/gcc.cmake
 create mode 100644 tools/polly/compiler/gcc48.cmake
 create mode 100644 tools/polly/compiler/xcode.cmake
 create mode 100644 tools/polly/custom-libcxx.cmake
 create mode 100644 tools/polly/cxx11.cmake
 create mode 100644 tools/polly/cxx17.cmake
 create mode 100644 tools/polly/cygwin.cmake
 create mode 100644 tools/polly/default.cmake
 create mode 100644 tools/polly/docs/Makefile
 create mode 100644 tools/polly/docs/_fixme/Building-libcxx.md
 create mode 100644 tools/polly/docs/_fixme/Jenkins-(build-bot,-PR).md
 create mode 100644 tools/polly/docs/_fixme/Jenkins-(build-bot,-integration).md
 create mode 100644 tools/polly/docs/_fixme/Jenkins-(creating-user-on-linux-ubuntu).md
 create mode 100644 tools/polly/docs/_fixme/Jenkins-(creating-user-on-mac).md
 create mode 100644 tools/polly/docs/_fixme/Jenkins.md
 create mode 100644 tools/polly/docs/_fixme/Toolchain-list.md
 create mode 100644 tools/polly/docs/_fixme/Travis-CI-AppVeyor-support-table.md
 create mode 100644 tools/polly/docs/_fixme/Used-variables.md
 create mode 100644 tools/polly/docs/conf.py
 create mode 100644 tools/polly/docs/index.rst
 create mode 100755 tools/polly/docs/jenkins.sh
 create mode 100755 tools/polly/docs/make.sh
 create mode 100644 tools/polly/docs/requirements.txt
 create mode 100644 tools/polly/docs/screens/ios-team-id.png
 create mode 100644 tools/polly/docs/spelling.txt
 create mode 100644 tools/polly/docs/toolchains.rst
 create mode 100644 tools/polly/docs/toolchains/android.rst
 create mode 100644 tools/polly/docs/toolchains/android/developer-notes.rst
 create mode 100644 tools/polly/docs/toolchains/android/old.rst
 create mode 100644 tools/polly/docs/toolchains/clang-omp.rst
 create mode 100644 tools/polly/docs/toolchains/gcc-musl.rst
 create mode 100644 tools/polly/docs/toolchains/ios.rst
 create mode 100644 tools/polly/docs/toolchains/ios/bundle-id.rst
 create mode 100644 tools/polly/docs/toolchains/ios/errors/polly_ios_bundle_identifier.rst
 create mode 100644 tools/polly/docs/toolchains/ios/errors/polly_ios_development_team.rst
 create mode 100644 tools/polly/docs/toolchains/ios/errors/signing-request-development-team.rst
 create mode 100644 tools/polly/docs/toolchains/ios/screens/01_new_project.png
 create mode 100644 tools/polly/docs/toolchains/ios/screens/02_single_view_app.png
 create mode 100644 tools/polly/docs/toolchains/ios/screens/03_project_options.png
 create mode 100644 tools/polly/docs/toolchains/ios/screens/04_bundle_identifier.png
 create mode 100644 tools/polly/docs/toolchains/ios/screens/05_run_app.png
 create mode 100644 tools/polly/docs/toolchains/ios/screens/bad_bundle_id.png
 create mode 100644 tools/polly/docs/toolchains/linux-mingw-w64.rst
 create mode 100644 tools/polly/docs/toolchains/raspberry-pi.rst
 create mode 100644 tools/polly/emscripten-cxx11.cmake
 create mode 100644 tools/polly/emscripten-cxx14.cmake
 create mode 100644 tools/polly/emscripten-cxx17.cmake
 create mode 100644 tools/polly/examples/01-executable/CMakeLists.txt
 create mode 100644 tools/polly/examples/01-executable/main.cpp
 create mode 100644 tools/polly/examples/02-library/CMakeLists.txt
 create mode 100644 tools/polly/examples/02-library/foo.cpp
 create mode 100755 tools/polly/examples/02-library/main.cpp
 create mode 100644 tools/polly/examples/03-shared-link/CMakeLists.txt
 create mode 100644 tools/polly/examples/03-shared-link/boo.cpp
 create mode 100644 tools/polly/examples/03-shared-link/foo.cpp
 create mode 100755 tools/polly/examples/03-shared-link/main.cpp
 create mode 100644 tools/polly/examples/04-framework/CMakeLists.txt
 create mode 100644 tools/polly/examples/04-framework/foo.cpp
 create mode 100644 tools/polly/find/FindLibcxx.cmake
 create mode 100644 tools/polly/flags/32bit.cmake
 create mode 100644 tools/polly/flags/arm-linux-gnueabihf.cmake
 create mode 100644 tools/polly/flags/bitcode.cmake
 create mode 100644 tools/polly/flags/c11.cmake
 create mode 100644 tools/polly/flags/clang-tidy.cmake
 create mode 100644 tools/polly/flags/cxx11.cmake
 create mode 100644 tools/polly/flags/cxx14.cmake
 create mode 100644 tools/polly/flags/cxx17-gnu.cmake
 create mode 100644 tools/polly/flags/cxx17.cmake
 create mode 100644 tools/polly/flags/cxx20.cmake
 create mode 100644 tools/polly/flags/cxx98.cmake
 create mode 100644 tools/polly/flags/data-sections.cmake
 create mode 100644 tools/polly/flags/fconcepts.cmake
 create mode 100644 tools/polly/flags/fpic.cmake
 create mode 100644 tools/polly/flags/function-sections.cmake
 create mode 100644 tools/polly/flags/gnuxx11.cmake
 create mode 100644 tools/polly/flags/gold.cmake
 create mode 100644 tools/polly/flags/hardfloat.cmake
 create mode 100644 tools/polly/flags/hidden.cmake
 create mode 100644 tools/polly/flags/ios_nocodesign.cmake
 create mode 100644 tools/polly/flags/lld.cmake
 create mode 100644 tools/polly/flags/lto.cmake
 create mode 100644 tools/polly/flags/mtune_cortex-a15.cmake
 create mode 100644 tools/polly/flags/neon-vfpv4.cmake
 create mode 100644 tools/polly/flags/neon.cmake
 create mode 100644 tools/polly/flags/openwrt.cmake
 create mode 100644 tools/polly/flags/sanitize_address.cmake
 create mode 100644 tools/polly/flags/sanitize_leak.cmake
 create mode 100644 tools/polly/flags/sanitize_memory.cmake
 create mode 100644 tools/polly/flags/sanitize_thread.cmake
 create mode 100644 tools/polly/flags/static-std.cmake
 create mode 100644 tools/polly/flags/static.cmake
 create mode 100644 tools/polly/flags/vs-cxx14.cmake
 create mode 100644 tools/polly/flags/vs-cxx17.cmake
 create mode 100644 tools/polly/flags/vs-mt.cmake
 create mode 100644 tools/polly/flags/vs-nonpermissive.cmake
 create mode 100644 tools/polly/flags/vs-platform-win32.cmake
 create mode 100644 tools/polly/flags/vs-platform-x64.cmake
 create mode 100644 tools/polly/flags/vs-version-14-11.cmake
 create mode 100644 tools/polly/flags/vs-z7.cmake
 create mode 100644 tools/polly/flags/vs-zw.cmake
 create mode 100644 tools/polly/gcc-32bit-pic.cmake
 create mode 100644 tools/polly/gcc-32bit.cmake
 create mode 100644 tools/polly/gcc-4-8-c11.cmake
 create mode 100644 tools/polly/gcc-4-8-pic-hid-sections-cxx11-c11.cmake
 create mode 100644 tools/polly/gcc-4-8-pic-hid-sections.cmake
 create mode 100644 tools/polly/gcc-4-8-pic.cmake
 create mode 100644 tools/polly/gcc-4-8.cmake
 create mode 100644 tools/polly/gcc-5-cxx14-c11.cmake
 create mode 100644 tools/polly/gcc-5-pic-hid-sections-lto.cmake
 create mode 100644 tools/polly/gcc-5-pic-hid-sections.cmake
 create mode 100644 tools/polly/gcc-5.cmake
 create mode 100644 tools/polly/gcc-6-32bit-cxx14.cmake
 create mode 100644 tools/polly/gcc-7-cxx11-pic.cmake
 create mode 100644 tools/polly/gcc-7-cxx14-pic.cmake
 create mode 100644 tools/polly/gcc-7-cxx14.cmake
 create mode 100644 tools/polly/gcc-7-cxx17-concepts.cmake
 create mode 100644 tools/polly/gcc-7-cxx17-gnu.cmake
 create mode 100644 tools/polly/gcc-7-cxx17-pic.cmake
 create mode 100644 tools/polly/gcc-7-cxx17.cmake
 create mode 100644 tools/polly/gcc-7-pic-hid-sections-lto.cmake
 create mode 100644 tools/polly/gcc-7.cmake
 create mode 100644 tools/polly/gcc-8-cxx14-fpic.cmake
 create mode 100644 tools/polly/gcc-8-cxx14.cmake
 create mode 100644 tools/polly/gcc-8-cxx17-concepts.cmake
 create mode 100644 tools/polly/gcc-8-cxx17-fpic.cmake
 create mode 100644 tools/polly/gcc-8-cxx17.cmake
 create mode 100644 tools/polly/gcc-c11.cmake
 create mode 100644 tools/polly/gcc-cxx14-c11.cmake
 create mode 100644 tools/polly/gcc-cxx17-c11.cmake
 create mode 100644 tools/polly/gcc-cxx98.cmake
 create mode 100644 tools/polly/gcc-gold.cmake
 create mode 100644 tools/polly/gcc-hid-fpic.cmake
 create mode 100644 tools/polly/gcc-hid.cmake
 create mode 100644 tools/polly/gcc-lto.cmake
 create mode 100644 tools/polly/gcc-musl.cmake
 create mode 100644 tools/polly/gcc-ninja.cmake
 create mode 100644 tools/polly/gcc-pic-hid-sections-lto.cmake
 create mode 100644 tools/polly/gcc-pic-hid-sections.cmake
 create mode 100644 tools/polly/gcc-pic.cmake
 create mode 100644 tools/polly/gcc-static-std.cmake
 create mode 100644 tools/polly/gcc-static.cmake
 create mode 100644 tools/polly/gcc.cmake
 create mode 100644 tools/polly/ios-10-0-arm64-dep-8-0-hid-sections.cmake
 create mode 100644 tools/polly/ios-10-0-arm64.cmake
 create mode 100644 tools/polly/ios-10-0-armv7.cmake
 create mode 100644 tools/polly/ios-10-0-dep-8-0-hid-sections.cmake
 create mode 100644 tools/polly/ios-10-0-wo-armv7s.cmake
 create mode 100644 tools/polly/ios-10-0.cmake
 create mode 100644 tools/polly/ios-10-1-arm64-dep-8-0-hid-sections.cmake
 create mode 100644 tools/polly/ios-10-1-arm64.cmake
 create mode 100644 tools/polly/ios-10-1-armv7.cmake
 create mode 100644 tools/polly/ios-10-1-dep-8-0-hid-sections.cmake
 create mode 100644 tools/polly/ios-10-1-dep-8-0-libcxx-hid-sections-lto.cmake
 create mode 100644 tools/polly/ios-10-1-dep-8-0-libcxx-hid-sections.cmake
 create mode 100644 tools/polly/ios-10-1-wo-armv7s.cmake
 create mode 100644 tools/polly/ios-10-1.cmake
 create mode 100644 tools/polly/ios-10-2-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-10-2-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-10-2.cmake
 create mode 100644 tools/polly/ios-10-3-arm64.cmake
 create mode 100644 tools/polly/ios-10-3-armv7.cmake
 create mode 100644 tools/polly/ios-10-3-dep-8-0-bitcode.cmake
 create mode 100644 tools/polly/ios-10-3-dep-9-0-bitcode.cmake
 create mode 100644 tools/polly/ios-10-3-dep-9-3-i386-armv7.cmake
 create mode 100644 tools/polly/ios-10-3-dep-9-3-x86-64-arm64.cmake
 create mode 100644 tools/polly/ios-10-3-lto.cmake
 create mode 100644 tools/polly/ios-10-3.cmake
 create mode 100644 tools/polly/ios-11-0-dep-9-0-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-11-0-dep-9-0-device-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-11-0-dep-9-0-x86-64-arm64-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-11-0.cmake
 create mode 100644 tools/polly/ios-11-1-dep-9-0-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-11-1-dep-9-0-device-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-11-2-dep-9-0-device-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-11-2-dep-9-0-device-bitcode-nocxx.cmake
 create mode 100644 tools/polly/ios-11-2-dep-9-3-arm64-armv7.cmake
 create mode 100644 tools/polly/ios-11-3-dep-9-0-arm64.cmake
 create mode 100644 tools/polly/ios-11-3-dep-9-0-device-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-11-3-dep-9-0-device-bitcode-cxx17.cmake
 create mode 100644 tools/polly/ios-11-3-dep-9-0-device-bitcode-nocxx.cmake
 create mode 100644 tools/polly/ios-11-3-dep-9-0-device-bitcode.cmake
 create mode 100644 tools/polly/ios-11-3-dep-9-3-arm64-armv7.cmake
 create mode 100644 tools/polly/ios-11-4-dep-8-0-arm64-armv7-hid-sections-lto-cxx11.cmake
 create mode 100644 tools/polly/ios-11-4-dep-8-0-arm64-hid-sections-lto-cxx11.cmake
 create mode 100644 tools/polly/ios-11-4-dep-9-0-device-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-11-4-dep-9-0-device-bitcode-nocxx.cmake
 create mode 100644 tools/polly/ios-11-4-dep-9-3-arm64-armv7.cmake
 create mode 100644 tools/polly/ios-11-4-dep-9-3-arm64-hid-sections-lto-cxx11.cmake
 create mode 100644 tools/polly/ios-11-4-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-11-4-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-11-4-dep-9-3.cmake
 create mode 100644 tools/polly/ios-11-4-dep-9-4-arm64.cmake
 create mode 100644 tools/polly/ios-12-0-dep-11-0-arm64.cmake
 create mode 100644 tools/polly/ios-12-0-dep-9-0-device-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-12-1-dep-11-0-arm64.cmake
 create mode 100644 tools/polly/ios-12-1-dep-12-0-arm64-cxx17.cmake
 create mode 100644 tools/polly/ios-12-1-dep-9-0-device-bitcode-cxx14.cmake
 create mode 100644 tools/polly/ios-12-1-dep-9-0-device-bitcode-cxx17.cmake
 create mode 100644 tools/polly/ios-12-1-dep-9-3-arm64-bitcode.cmake
 create mode 100644 tools/polly/ios-12-1-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-12-1-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-12-1-dep-9-3-x86-64-arm64.cmake
 create mode 100644 tools/polly/ios-12-1-dep-9-3.cmake
 create mode 100644 tools/polly/ios-12-2-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-12-3-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-13-0-dep-10-0-arm64-bitcode-cxx17.cmake
 create mode 100644 tools/polly/ios-13-0-dep-11-0-arm64-bitcode-cxx17.cmake
 create mode 100644 tools/polly/ios-13-0-dep-9-3-arm64-bitcode.cmake
 create mode 100644 tools/polly/ios-13-0-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-13-2-dep-10-0-arm64-bitcode-cxx17.cmake
 create mode 100644 tools/polly/ios-13-2-dep-9-3-arm64-bitcode.cmake
 create mode 100644 tools/polly/ios-13-2-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-13-2-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-13-2-dep-9-3-armv7s.cmake
 create mode 100644 tools/polly/ios-13-2-dep-9-3-device-bitcode-cxx14.cmake
 create mode 100644 tools/polly/ios-13-2-dep-9-3-device-cxx14.cmake
 create mode 100644 tools/polly/ios-13-3-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-13-3-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-13-3-dep-9-3-armv7s.cmake
 create mode 100644 tools/polly/ios-13-3-dep-9-3-device-bitcode-cxx14.cmake
 create mode 100644 tools/polly/ios-13-3-dep-9-3-device-cxx14.cmake
 create mode 100644 tools/polly/ios-13-4-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-13-4-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-13-4-dep-9-3-armv7s.cmake
 create mode 100644 tools/polly/ios-13-4-dep-9-3-device-bitcode-cxx14.cmake
 create mode 100644 tools/polly/ios-13-4-dep-9-3-device-cxx14.cmake
 create mode 100644 tools/polly/ios-13-5-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-13-5-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-13-5-dep-9-3-armv7s.cmake
 create mode 100644 tools/polly/ios-13-5-dep-9-3-device-bitcode-cxx14.cmake
 create mode 100644 tools/polly/ios-13-5-dep-9-3-device-cxx14.cmake
 create mode 100644 tools/polly/ios-13-6-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-13-6-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-13-6-dep-9-3-armv7s.cmake
 create mode 100644 tools/polly/ios-13-6-dep-9-3-device-bitcode-cxx14.cmake
 create mode 100644 tools/polly/ios-13-6-dep-9-3-device-cxx14.cmake
 create mode 100644 tools/polly/ios-14-0-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-14-0-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-14-0-dep-9-3-armv7s.cmake
 create mode 100644 tools/polly/ios-14-0-dep-9-3-device-bitcode-cxx14.cmake
 create mode 100644 tools/polly/ios-14-0-dep-9-3-device-cxx14.cmake
 create mode 100644 tools/polly/ios-14-2-dep-10-0-arm64.cmake
 create mode 100644 tools/polly/ios-14-2-dep-10-0-armv7.cmake
 create mode 100644 tools/polly/ios-14-2-dep-10-0-armv7s.cmake
 create mode 100644 tools/polly/ios-14-2-dep-10-0-device-bitcode-cxx14.cmake
 create mode 100644 tools/polly/ios-14-2-dep-10-0-device-cxx14.cmake
 create mode 100644 tools/polly/ios-14-3-dep-10-0-arm64.cmake
 create mode 100644 tools/polly/ios-14-3-dep-10-0-armv7.cmake
 create mode 100644 tools/polly/ios-14-3-dep-10-0-armv7s.cmake
 create mode 100644 tools/polly/ios-14-3-dep-10-0-device-bitcode-cxx14.cmake
 create mode 100644 tools/polly/ios-14-3-dep-10-0-device-cxx14.cmake
 create mode 100644 tools/polly/ios-14-4-dep-10-0-arm64.cmake
 create mode 100644 tools/polly/ios-14-4-dep-10-0-armv7.cmake
 create mode 100644 tools/polly/ios-14-4-dep-10-0-armv7s.cmake
 create mode 100644 tools/polly/ios-14-4-dep-10-0-device-bitcode-cxx14.cmake
 create mode 100644 tools/polly/ios-14-4-dep-10-0-device-cxx14.cmake
 create mode 100644 tools/polly/ios-7-0.cmake
 create mode 100644 tools/polly/ios-7-1.cmake
 create mode 100644 tools/polly/ios-8-0.cmake
 create mode 100644 tools/polly/ios-8-1.cmake
 create mode 100644 tools/polly/ios-8-2-arm64-hid.cmake
 create mode 100644 tools/polly/ios-8-2-arm64.cmake
 create mode 100644 tools/polly/ios-8-2-cxx98.cmake
 create mode 100644 tools/polly/ios-8-2-i386-arm64.cmake
 create mode 100644 tools/polly/ios-8-2.cmake
 create mode 100644 tools/polly/ios-8-4-arm64.cmake
 create mode 100644 tools/polly/ios-8-4-armv7.cmake
 create mode 100644 tools/polly/ios-8-4-armv7s.cmake
 create mode 100644 tools/polly/ios-8-4-hid.cmake
 create mode 100644 tools/polly/ios-8-4.cmake
 create mode 100644 tools/polly/ios-9-0-armv7.cmake
 create mode 100644 tools/polly/ios-9-0-dep-7-0-armv7.cmake
 create mode 100644 tools/polly/ios-9-0-i386-armv7.cmake
 create mode 100644 tools/polly/ios-9-0-wo-armv7s.cmake
 create mode 100644 tools/polly/ios-9-0.cmake
 create mode 100644 tools/polly/ios-9-1-arm64.cmake
 create mode 100644 tools/polly/ios-9-1-armv7.cmake
 create mode 100644 tools/polly/ios-9-1-dep-7-0-armv7.cmake
 create mode 100644 tools/polly/ios-9-1-dep-8-0-hid.cmake
 create mode 100644 tools/polly/ios-9-1-hid.cmake
 create mode 100644 tools/polly/ios-9-1.cmake
 create mode 100644 tools/polly/ios-9-2-arm64.cmake
 create mode 100644 tools/polly/ios-9-2-armv7.cmake
 create mode 100644 tools/polly/ios-9-2-hid-sections.cmake
 create mode 100644 tools/polly/ios-9-2-hid.cmake
 create mode 100644 tools/polly/ios-9-2.cmake
 create mode 100644 tools/polly/ios-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-9-3-wo-armv7s.cmake
 create mode 100644 tools/polly/ios-9-3.cmake
 create mode 100755 tools/polly/ios-bitcode.cmake
 create mode 100644 tools/polly/ios-cxx17.cmake
 create mode 100644 tools/polly/ios-dep-10-0-bitcode-cxx17.cmake
 create mode 100644 tools/polly/ios-dep-11-0-bitcode-cxx17.cmake
 create mode 100644 tools/polly/ios-dep-12-0-bitcode-cxx17.cmake
 create mode 100644 tools/polly/ios-dep-8-0-arm64-armv7-hid-sections-cxx11.cmake
 create mode 100644 tools/polly/ios-dep-8-0-arm64-armv7-hid-sections-lto-cxx11.cmake
 create mode 100644 tools/polly/ios-dep-8-0-arm64-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-0-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-0-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-0-wo-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-0.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-1-arm64-dep-9-0-device-libcxx-hid-sections-lto.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-1-arm64-dep-9-0-device-libcxx-hid-sections.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-1-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-1-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-1-dep-8-0-device-libcxx-hid-sections-lto.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-1-dep-8-0-libcxx-hid-sections-lto.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-1-dep-9-0-device-libcxx-hid-sections-lto.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-1-wo-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-1.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-2.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-3-arm64-dep-9-0-device-libcxx-hid-sections.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-3-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-3-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-3-cxx14.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-3-dep-9-0-bitcode.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-3-wo-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign-10-3.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-0-arm64-dep-9-0-device-libcxx-hid-sections.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-0-dep-9-0-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-0.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-1-dep-9-0-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-1-dep-9-0-wo-armv7s-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-1.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-2-dep-8-0-wo-armv7s-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-2-dep-9-0-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-2-dep-9-3-arm64-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-2-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-2-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-2-dep-9-3-i386-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-2-dep-9-3.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-2.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-3-dep-9-0-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-3-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-3-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-3-dep-9-3.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-4-dep-9-0-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-4-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-4-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-11-4-dep-9-3.cmake
 create mode 100644 tools/polly/ios-nocodesign-12-0-dep-10-0-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-12-0-dep-9-0-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-12-1-dep-9-0-bitcode-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-12-1-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-12-1.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-0-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-2-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-2-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-2-dep-9-3-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-2-dep-9-3-device-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-2-dep-9-3-device.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-2-dep-9-3.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-5-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-5-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-5-dep-9-3-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-5-dep-9-3-device-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-5-dep-9-3-device.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-5-dep-9-3.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-6-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-6-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-6-dep-9-3-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-6-dep-9-3-device-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-6-dep-9-3-device.cmake
 create mode 100644 tools/polly/ios-nocodesign-13-6-dep-9-3.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-0-dep-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-0-dep-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-0-dep-9-3-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-0-dep-9-3-device-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-0-dep-9-3-device.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-0-dep-9-3.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-2-dep-10-0-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-2-dep-10-0-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-2-dep-10-0-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-2-dep-10-0-device-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-2-dep-10-0-device.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-2-dep-10-0.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-3-dep-10-0-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-3-dep-10-0-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-3-dep-10-0-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-3-dep-10-0-device-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-3-dep-10-0-device.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-3-dep-10-0.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-4-dep-10-0-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-4-dep-10-0-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-4-dep-10-0-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-4-dep-10-0-device-cxx11.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-4-dep-10-0-device.cmake
 create mode 100644 tools/polly/ios-nocodesign-14-4-dep-10-0.cmake
 create mode 100644 tools/polly/ios-nocodesign-8-1.cmake
 create mode 100644 tools/polly/ios-nocodesign-8-4.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-1-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-1-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-1.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-2-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-2-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-2.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-3-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-3-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-3-device-hid-sections.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-3-device.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-3-wo-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign-9-3.cmake
 create mode 100644 tools/polly/ios-nocodesign-arm64.cmake
 create mode 100644 tools/polly/ios-nocodesign-armv7.cmake
 create mode 100644 tools/polly/ios-nocodesign-dep-9-0-cxx14.cmake
 create mode 100644 tools/polly/ios-nocodesign-hid-sections.cmake
 create mode 100644 tools/polly/ios-nocodesign-wo-armv7s.cmake
 create mode 100644 tools/polly/ios-nocodesign.cmake
 create mode 100644 tools/polly/ios.cmake
 create mode 100644 tools/polly/libcxx-fpic-hid-sections.cmake
 create mode 100644 tools/polly/libcxx-hid-fpic.cmake
 create mode 100644 tools/polly/libcxx-hid-sections.cmake
 create mode 100644 tools/polly/libcxx-hid.cmake
 create mode 100644 tools/polly/libcxx-no-sdk.cmake
 create mode 100644 tools/polly/libcxx.cmake
 create mode 100644 tools/polly/libcxx14.cmake
 create mode 100644 tools/polly/library/std/libcxx.cmake
 create mode 100644 tools/polly/library/std/libstdcxx.cmake
 create mode 100644 tools/polly/library/std/nolibs.cmake
 create mode 100644 tools/polly/linux-gcc-armhf-neon-vfpv4.cmake
 create mode 100644 tools/polly/linux-gcc-armhf-neon.cmake
 create mode 100644 tools/polly/linux-gcc-armhf.cmake
 create mode 100644 tools/polly/linux-gcc-jetson-tk1.cmake
 create mode 100644 tools/polly/linux-gcc-x64.cmake
 create mode 100644 tools/polly/linux-mingw-w32-cxx14.cmake
 create mode 100644 tools/polly/linux-mingw-w32.cmake
 create mode 100644 tools/polly/linux-mingw-w64-cxx14.cmake
 create mode 100644 tools/polly/linux-mingw-w64-cxx98.cmake
 create mode 100644 tools/polly/linux-mingw-w64-gnuxx11.cmake
 create mode 100644 tools/polly/linux-mingw-w64.cmake
 create mode 100644 tools/polly/mingw-c11.cmake
 create mode 100644 tools/polly/mingw-cxx14.cmake
 create mode 100644 tools/polly/mingw-cxx17.cmake
 create mode 100644 tools/polly/mingw.cmake
 create mode 100644 tools/polly/msys-cxx14.cmake
 create mode 100644 tools/polly/msys-cxx17.cmake
 create mode 100644 tools/polly/msys.cmake
 create mode 100644 tools/polly/ninja-gcc-7-cxx17-concepts.cmake
 create mode 100644 tools/polly/ninja-gcc-8-cxx17-concepts.cmake
 create mode 100644 tools/polly/ninja-vs-12-2013-win64.cmake
 create mode 100644 tools/polly/ninja-vs-14-2015-win64.cmake
 create mode 100644 tools/polly/ninja-vs-15-2017-win64-cxx17-nonpermissive.cmake
 create mode 100644 tools/polly/ninja-vs-15-2017-win64-cxx17.cmake
 create mode 100644 tools/polly/ninja-vs-15-2017-win64.cmake
 create mode 100644 tools/polly/nmake-vs-12-2013-win64.cmake
 create mode 100644 tools/polly/nmake-vs-12-2013.cmake
 create mode 100644 tools/polly/nmake-vs-15-2017-win64-cxx17-nonpermissive.cmake
 create mode 100644 tools/polly/nmake-vs-15-2017-win64-cxx17.cmake
 create mode 100644 tools/polly/nmake-vs-15-2017-win64.cmake
 create mode 100644 tools/polly/openbsd-egcc-cxx11-static-std.cmake
 create mode 100644 tools/polly/os/android.cmake
 create mode 100644 tools/polly/os/cygwin.cmake
 create mode 100644 tools/polly/os/iphone-default-sdk.cmake
 create mode 100644 tools/polly/os/iphone.cmake
 create mode 100644 tools/polly/os/osx.cmake
 create mode 100644 tools/polly/os/raspberry-pi-hardfloat.cmake
 create mode 100644 tools/polly/os/raspberry-pi1.cmake
 create mode 100644 tools/polly/os/raspberry-pi2.cmake
 create mode 100644 tools/polly/os/raspberry-pi3.cmake
 create mode 100644 tools/polly/os/rpi-sysroot.cmake
 create mode 100644 tools/polly/os/vc-mdd-android.cmake
 create mode 100644 tools/polly/osx-10-10-dep-10-7.cmake
 create mode 100644 tools/polly/osx-10-10-dep-10-9-make.cmake
 create mode 100644 tools/polly/osx-10-10.cmake
 create mode 100644 tools/polly/osx-10-11-hid-sections-lto.cmake
 create mode 100644 tools/polly/osx-10-11-hid-sections.cmake
 create mode 100644 tools/polly/osx-10-11-lto.cmake
 create mode 100644 tools/polly/osx-10-11-make.cmake
 create mode 100644 tools/polly/osx-10-11-sanitize-address.cmake
 create mode 100644 tools/polly/osx-10-11.cmake
 create mode 100644 tools/polly/osx-10-12-cxx14.cmake
 create mode 100644 tools/polly/osx-10-12-cxx17.cmake
 create mode 100644 tools/polly/osx-10-12-cxx98.cmake
 create mode 100644 tools/polly/osx-10-12-dep-10-10-lto.cmake
 create mode 100644 tools/polly/osx-10-12-dep-10-10.cmake
 create mode 100644 tools/polly/osx-10-12-hid-sections.cmake
 create mode 100644 tools/polly/osx-10-12-lto.cmake
 create mode 100644 tools/polly/osx-10-12-make.cmake
 create mode 100644 tools/polly/osx-10-12-ninja.cmake
 create mode 100644 tools/polly/osx-10-12-sanitize-address-hid-sections.cmake
 create mode 100644 tools/polly/osx-10-12-sanitize-address.cmake
 create mode 100644 tools/polly/osx-10-12.cmake
 create mode 100644 tools/polly/osx-10-13-cxx14.cmake
 create mode 100644 tools/polly/osx-10-13-cxx17.cmake
 create mode 100644 tools/polly/osx-10-13-dep-10-10-cxx14.cmake
 create mode 100644 tools/polly/osx-10-13-dep-10-10-cxx17.cmake
 create mode 100644 tools/polly/osx-10-13-dep-10-10.cmake
 create mode 100644 tools/polly/osx-10-13-dep-10-12-cxx14.cmake
 create mode 100644 tools/polly/osx-10-13-dep-10-12-cxx17.cmake
 create mode 100644 tools/polly/osx-10-13-dep-10-12.cmake
 create mode 100644 tools/polly/osx-10-13-i386-cxx14.cmake
 create mode 100644 tools/polly/osx-10-13-make-cxx14.cmake
 create mode 100644 tools/polly/osx-10-13.cmake
 create mode 100644 tools/polly/osx-10-14-cxx14.cmake
 create mode 100644 tools/polly/osx-10-14-cxx17.cmake
 create mode 100644 tools/polly/osx-10-14-dep-10-10-cxx14.cmake
 create mode 100644 tools/polly/osx-10-14-dep-10-10-cxx17.cmake
 create mode 100644 tools/polly/osx-10-14-dep-10-10.cmake
 create mode 100644 tools/polly/osx-10-14-dep-10-12-cxx14.cmake
 create mode 100644 tools/polly/osx-10-14-dep-10-12-cxx17.cmake
 create mode 100644 tools/polly/osx-10-14-dep-10-12.cmake
 create mode 100644 tools/polly/osx-10-14.cmake
 create mode 100644 tools/polly/osx-10-15-cxx17.cmake
 create mode 100644 tools/polly/osx-10-15-dep-10-10-cxx14.cmake
 create mode 100644 tools/polly/osx-10-15-dep-10-10-cxx17.cmake
 create mode 100644 tools/polly/osx-10-15-dep-10-10.cmake
 create mode 100644 tools/polly/osx-10-15-dep-10-12-cxx17.cmake
 create mode 100644 tools/polly/osx-10-15.cmake
 create mode 100644 tools/polly/osx-10-7.cmake
 create mode 100644 tools/polly/osx-10-8.cmake
 create mode 100644 tools/polly/osx-10-9.cmake
 create mode 100644 tools/polly/osx-11-0-cxx17.cmake
 create mode 100644 tools/polly/osx-11-0-dep-10-10-cxx17.cmake
 create mode 100644 tools/polly/osx-11-0.cmake
 create mode 100644 tools/polly/osx-11-1-cxx17.cmake
 create mode 100644 tools/polly/osx-11-1-dep-10-10-cxx17.cmake
 create mode 100644 tools/polly/osx-11-1.cmake
 create mode 100644 tools/polly/raspberrypi1-cxx11-pic-static-std.cmake
 create mode 100644 tools/polly/raspberrypi1-cxx11-pic.cmake
 create mode 100644 tools/polly/raspberrypi1-cxx14-pic-static-std.cmake
 create mode 100644 tools/polly/raspberrypi2-cxx11-pic.cmake
 create mode 100644 tools/polly/raspberrypi2-cxx11.cmake
 create mode 100644 tools/polly/raspberrypi3-clang-cxx11.cmake
 create mode 100644 tools/polly/raspberrypi3-clang-cxx14-pic.cmake
 create mode 100644 tools/polly/raspberrypi3-clang-cxx14.cmake
 create mode 100644 tools/polly/raspberrypi3-cxx11.cmake
 create mode 100644 tools/polly/raspberrypi3-cxx14.cmake
 create mode 100644 tools/polly/raspberrypi3-gcc-pic-hid-sections.cmake
 create mode 100644 tools/polly/sanitize-address-cxx17-pic.cmake
 create mode 100644 tools/polly/sanitize-address-cxx17.cmake
 create mode 100644 tools/polly/sanitize-address.cmake
 create mode 100644 tools/polly/sanitize-leak-cxx17-pic.cmake
 create mode 100644 tools/polly/sanitize-leak-cxx17.cmake
 create mode 100644 tools/polly/sanitize-leak.cmake
 create mode 100644 tools/polly/sanitize-memory.cmake
 create mode 100644 tools/polly/sanitize-thread-cxx17-pic.cmake
 create mode 100644 tools/polly/sanitize-thread-cxx17.cmake
 create mode 100644 tools/polly/sanitize-thread.cmake
 create mode 100644 tools/polly/scripts/Info.plist
 create mode 100644 tools/polly/scripts/NoCodeSign.xcconfig
 create mode 100755 tools/polly/scripts/clang-analyze.sh
 create mode 100755 tools/polly/scripts/clangxx-analyze.sh
 create mode 100644 tools/polly/utilities/polly_add_cache_flag.cmake
 create mode 100644 tools/polly/utilities/polly_clear_environment_variables.cmake
 create mode 100644 tools/polly/utilities/polly_common.cmake
 create mode 100644 tools/polly/utilities/polly_fatal_error.cmake
 create mode 100644 tools/polly/utilities/polly_init.cmake
 create mode 100644 tools/polly/utilities/polly_ios_bundle_identifier.cmake
 create mode 100644 tools/polly/utilities/polly_ios_development_team.cmake
 create mode 100644 tools/polly/utilities/polly_module_path.cmake
 create mode 100644 tools/polly/utilities/polly_status_debug.cmake
 create mode 100644 tools/polly/utilities/polly_status_print.cmake
 create mode 100644 tools/polly/vs-10-2010.cmake
 create mode 100644 tools/polly/vs-11-2012-arm.cmake
 create mode 100644 tools/polly/vs-11-2012-win64.cmake
 create mode 100644 tools/polly/vs-11-2012.cmake
 create mode 100644 tools/polly/vs-12-2013-arm.cmake
 create mode 100644 tools/polly/vs-12-2013-mt.cmake
 create mode 100644 tools/polly/vs-12-2013-win64.cmake
 create mode 100644 tools/polly/vs-12-2013-xp.cmake
 create mode 100644 tools/polly/vs-12-2013.cmake
 create mode 100644 tools/polly/vs-14-2015-arm.cmake
 create mode 100644 tools/polly/vs-14-2015-sdk-8-1.cmake
 create mode 100644 tools/polly/vs-14-2015-win64-sdk-8-1.cmake
 create mode 100644 tools/polly/vs-14-2015-win64.cmake
 create mode 100644 tools/polly/vs-14-2015.cmake
 create mode 100644 tools/polly/vs-15-2017-cxx14-mt.cmake
 create mode 100644 tools/polly/vs-15-2017-cxx17.cmake
 create mode 100644 tools/polly/vs-15-2017-mt.cmake
 create mode 100644 tools/polly/vs-15-2017-store-10-zw.cmake
 create mode 100644 tools/polly/vs-15-2017-win64-cxx14-mt.cmake
 create mode 100644 tools/polly/vs-15-2017-win64-cxx14.cmake
 create mode 100644 tools/polly/vs-15-2017-win64-cxx17-nonpermissive.cmake
 create mode 100644 tools/polly/vs-15-2017-win64-cxx17.cmake
 create mode 100644 tools/polly/vs-15-2017-win64-llvm-vs2014.cmake
 create mode 100644 tools/polly/vs-15-2017-win64-llvm.cmake
 create mode 100644 tools/polly/vs-15-2017-win64-mt.cmake
 create mode 100644 tools/polly/vs-15-2017-win64-store-10-cxx17.cmake
 create mode 100644 tools/polly/vs-15-2017-win64-store-10-zw.cmake
 create mode 100644 tools/polly/vs-15-2017-win64-version-14-11.cmake
 create mode 100644 tools/polly/vs-15-2017-win64-z7.cmake
 create mode 100644 tools/polly/vs-15-2017-win64.cmake
 create mode 100644 tools/polly/vs-15-2017.cmake
 create mode 100644 tools/polly/vs-16-2019-cxx14.cmake
 create mode 100644 tools/polly/vs-16-2019-cxx17.cmake
 create mode 100644 tools/polly/vs-16-2019-llvm-cxx17.cmake
 create mode 100644 tools/polly/vs-16-2019-win64-cxx14.cmake
 create mode 100644 tools/polly/vs-16-2019-win64-cxx17.cmake
 create mode 100644 tools/polly/vs-16-2019-win64-llvm-cxx17.cmake
 create mode 100644 tools/polly/vs-16-2019-win64.cmake
 create mode 100644 tools/polly/vs-16-2019.cmake
 create mode 100644 tools/polly/vs-8-2005.cmake
 create mode 100644 tools/polly/vs-9-2008.cmake
 create mode 100644 tools/polly/xcode-cxx17.cmake
 create mode 100644 tools/polly/xcode-cxx98.cmake
 create mode 100644 tools/polly/xcode-gcc.cmake
 create mode 100644 tools/polly/xcode-hid-sections.cmake
 create mode 100644 tools/polly/xcode-nocxx.cmake
 create mode 100644 tools/polly/xcode-sections.cmake
 create mode 100644 tools/polly/xcode.cmake
Успешно перемещён и обновлён refs/heads/master.
 ~/ Kinpach/workspace/TimpLab08$ git push origin master
Username for 'https://github.com':  Kinpach
Password for 'https:// Kinpach@github.com': 
Перечисление объектов: 847, готово.
Подсчет объектов: 100% (847/847), готово.
При сжатии изменений используется до 8 потоков
Сжатие объектов: 100% (831/831), готово.
Запись объектов: 100% (845/845), 537.80 КиБ | 6.25 МиБ/с, готово.
Всего 845 (изменений 591), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
remote: Resolving deltas: 100% (591/591), completed with 1 local object.
To https://github.com/ Kinpach/TimpLab08
   b618ab9..fc072b9  master -> master
 ~/ Kinpach/workspace/TimpLab08$
