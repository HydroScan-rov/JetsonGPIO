# Copyright (c) 2012-2017 Ben Croston ben@croston.org.
# Copyright (c) 2019, NVIDIA CORPORATION.
# Copyright (c) 2019 Jueon Park(pjueon) bluegbg@gmail.com.
# 
# Permission is hereby granted, free of charge, to any person obtaining a
# copy of this software and associated documentation files (the "Software"),
# to deal in the Software without restriction, including without limitation
# the rights to use, copy, modify, merge, publish, distribute, sublicense,
# and/or sell copies of the Software, and to permit persons to whom the
# Software is furnished to do so, subject to the following conditions:
# 
# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.
# 
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. GPIO_IN NO EVENT SHALL
# THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER GPIO_IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
# FROM, GPIO_OUT OF OR GPIO_IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
# DEALINGS GPIO_IN THE SOFTWARE.

FROM nvcr.io/nvidia/l4t-base:r32.7.1 AS base

RUN apt-get update 
RUN apt-get install git build-essential cmake -y
RUN apt-get install -y python3-pip
RUN pip3 install Jetson.GPIO

# build 
FROM base AS builder

WORKDIR /source
RUN git clone https://github.com/pjueon/JetsonGPIO.git

WORKDIR /source/JetsonGPIO
RUN mkdir build

WORKDIR /source/JetsonGPIO/build
RUN cmake .. -DCMAKE_INSTALL_PREFIX=./install -DBUILD_EXAMPLES=ON -DJETSON_GPIO_POST_INSTALL=OFF
RUN cmake --build . --target install

# app
FROM base AS app 

# install JetsonGPIO
COPY --from=builder /source/JetsonGPIO/build/install/ /usr/local/
# COPY --from=builder /lib/udev/rules.d/99-gpio.rules /lib/udev/rules.d/99-gpio.rules

# copy samples and tests
COPY --from=builder /source/JetsonGPIO/build/samples/ /gpio-cpp/samples
COPY --from=builder /source/JetsonGPIO/build/tests/ /gpio-cpp/tests
